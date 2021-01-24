---
layout: post
title:  Pretty PDFs from Emacs Org-Mode via Chromium
date:   2021-01-24 15:00:00 -0400
---

Though I've been using emacs to for quite a while, and often write plain text documentation in it, I've never previously explored org-mode. Recently however, I had some table heavy documentation to write and export to HTML and PDF, and decided to finally learn org-mode. Exporting to a nice HTML document was not too much trouble, there are many fine CSS layouts on github to pick from. I ended up basing my format off of [orgcss](https://github.com/gongzhitaao/orgcss) with a few small modifications.

However when I tried to export to PDF, I hit a bit of a problem as  Org Mode exports to PDF by first exporting to LaTeX, then then on to PDF. I had to install about a gigabyte of packages on my OS in order to get this working, and when I did, I was rewarded with a document that looked pretty much like any computer science thesis or research paper from the past 40 years. LaTeX is of course, highly configurable, and I could tweak the visual output to be more to my liking, but I decided to go a different route. I figured since I already had HTML that I liked, why couldn't I just go Org -> HTML -> PDF, instead of Org -> LaTeX -> PDF.

I first tested this out by opening my HTML in chromium and just doing 'print PDF'. I was happy enough with the result, but didn't want to have to do this manually each time I wanted a PDF, so I set out to automate the process. My goal was a shell script could run on the command line that takes an Org file as input and outputs a PDF version in a single step. In addition, I wanted to be able to run this script right from an emacs buffer and export a PDF right from the buffer contents (without needing to save first). I also decided to add a flag to automatically pop the result up in a viewer for easy iterating while writing org files. I just `M-x org-to-pdf` and within a couple of seconds (longer for longer org files), I'm greeted by the rendered output of my work.

The script relies on emacs to do the Org->HTML conversion, and calls chromium to do the HTML->PDF conversion. Viewing of a PDF is handled by evince. The rest is just glue and error handling:

```sh
#!/bin/sh

display_help()
{
  echo "Usage: 2pdf.sh  [-v] [-k] <input file>" >&2
  echo "   <input file> can be .html or .org" >&2  
  echo "   -v - Open PDF in evince after creation. \
           (will first kill any open evince with the same file displayed" >&2  
  echo "   -k - Keep interim .html file when converting .org to .pdf"
  echo "   -h - Show this message" >&2
  echo  >&2
  echo "   PDF file will have same basename as input file \
           and .pdf extension"  >&2  
  echo "   Will read .org input from stdin if passed - as an input file. \
           In this case output file will be 2pdf.tmp.pdf" >&2  
  exit 0;
}

while getopts hvk opt
do
    case $opt in
        v)    viewPdf=1
              ;;
        k)    keepHtml=1
              ;;
        h)    display_help
              ;;
    esac
done
shift $(($OPTIND - 1))

if [ "$#" -ne 1 ]; then
  display_help
fi

if [ "$1" == "-" ]; then
    filename=2pdf.tmp.org
    cat > $filename
else
    filename=$1
fi

if ! [ -e "$filename" ]; then
  echo "$filename not found" >&2
  exit 1
fi

BASE=`echo "${filename%.*}"`
EXT=${filename##*\.}
PDF=${BASE}.pdf
HTML=${BASE}.html


case "$EXT" in
    html) 
	  HTML=$filename
          keepHtml=1
          ;;
    org)  emacs --batch --eval "(require 'org)" "$filename" \
          --funcall org-html-export-to-html || exit 1
	  ;;
    *)    echo " $filename : Unsupported Inputfile Type" >&2
          exit 1
          ;;
esac

chrome --disable-gpu --headless --print-to-pdf-no-header \
       --print-to-pdf="$PDF" "$HTML" || exit 2

if [ ! -z "$viewPdf" ]; then
  pkill -f "evince $PDF"
  nohup evince "$PDF"  >/dev/null 2>&1 &
fi

if [ -z "$keepHtml" ]; then
   rm "$HTML"
fi
```

With this I can call `2pdf.sh orgfile.org` to generate orgfile.pdf. If I add a `-k` onto the command line, I also get an orgfile.html, and a `-v` will open up the PDF viewer after creating the PDF. I also added a pkill to kill any existing viewers already showing the same file, so I didn't have to manually close them or end up with dozens when iterating. The script can also take org file input on stdin like `cat orgfile.org | 2pdf.sh - `. Finally if passed an HTML file instead of a org file, it only performs the HTML->PDF conversion (only .org format is supported on stdin though)

Creating an emacs function to call this was as simple as adding a few lines to my `.emacs`. I don't speak much lisp, so I based this off of [a recipe from "Mastering Emacs"](https://www.masteringemacs.org/article/executing-shell-commands-emacs).


```lisp
(defun org-to-pdf ()
  "Converts org buffer contents to pdf"
  (interactive)
  (shell-command-on-region
   (point-min)
   (point-max)
   "~/scripts/2pdf.sh -v -"
   nil
   nil
   "*PDF Conversion Error Buffer*"
   nil))
```

Its little weird that I'm calling a shell script from emacs which then itself runs a headless emacs to do it's work. I wrote the script so it can also take html as its input and just do the PDF conversion. At some point I should learn enough elisp to call `org-html-export-to-html`, extract the filename from the current buffer, replace the .org with .html, and then call 2pdf.sh on that, but this method works just fine for now.

One last little annoyance, which is always a problem when creating PDFs from HTML, is page breaks in dumb places, like right after a heading, or after the first line of a table. I've never found a good automated solution to this, but for Org Mode exports, I just manually put in html page breaks before the dumb place it would otherwise break. This means that if I further expand the document, I'd need to move my manual page breaks, but at least its a workaround. I put a macro in my Org files:

```
#+MACRO: new_page @@html:<p style="page-break-before: always">@@
```

I can then insert a manual page break with {% raw %} {{{new_page}}} {% endraw %}.







