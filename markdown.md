# Markdown + Pandoc

- The documents are written in markdown.
- Find a markdown cheatsheet here.

## Convert document into html

```sh
pandoc -s -f markdown -t html inputfile.md -o outputfile.html
```

## Convert a document into pdf

To convert a document into pdf, latex is necessary. The following packages need to be installed

- texlive
- texlive-latex
- texlive-xetex
- texlive-collection-latex
- texlive-collection-latexrecommended
- texlive-xetex-def
- texlive-collection-xetex

then it's as easy as

```sh
pandoc --pdf-engine=xelatex inputfile.md -o outputfile.md
```
	

## Add equations

- `$...$` for inline equations
- `$$...$$` for centred equations

## Set title

At the beginning of the document:
---
title: This is the title
...


## Set font

In the header:
---
mainfont: Arial
...

## Create a table of contents in the output

Table of contents can be created adding the option `--toc`. The command has to also include the -s option.
