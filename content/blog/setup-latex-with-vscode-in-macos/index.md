---
title: Setup Latex with VSCode in MacOS
date: "2019-11-17T22:12:03.284Z"
description: "Simple steps to setup Latex with VSCode in MacOS"
---

Recently, I need to update my resume, which is written in Latex.

Here is how I make it work on MacOS and VSCode.

- Install Latex compilers & libraries

(You need `brew` for this. If you haven't install it, follow the instruction in [here](https://brew.sh/) )

```shell
# We will use VSCode as editor, so `-no-gui` part comes.
brew cask install mactex-no-gui

# Update all Latex libraries
sudo tlmgr update --self
sudo tlmgr update --all

# (Optional) Update all TeX format.
# You will need this for Chinese support library - CTEX
sudo fmtutil-sys --all
```

- Install `LaTeX Workshop` extension in VSCode

So far, if you don't need Chinese support, you are all set and good to go.

However, if you do, there are a few more steps to do.

- Setup `LaTeX Workshop`

```json
// .vscode/setting.json
{
  "latex-workshop.latex.tools": [
    {
      "name": "xelatex",
      "command": "xelatex",
      "args": [
        "-synctex=1",
        "-interaction=nonstopmode",
        "-file-line-error",
        "-pdf",
        "%DOC%"
      ]
    },
    {
      "name": "latexmk",
      "command": "latexmk",
      "args": [
        "-synctex=1",
        "-interaction=nonstopmode",
        "-file-line-error",
        "-pdf",
        "%DOC%"
      ]
    },
    {
      "name": "pdflatex",
      "command": "pdflatex",
      "args": [
        "-synctex=1",
        "-interaction=nonstopmode",
        "-file-line-error",
        "%DOC%"
      ]
    },
    {
      "name": "bibtex",
      "command": "bibtex",
      "args": ["%DOCFILE%"]
    }
  ],
  "latex-workshop.latex.recipes": [
    {
      "name": "xelatex",
      "tools": ["xelatex"]
    },
    {
      "name": "pdflatex -> bibtex -> pdflatex*2",
      "tools": ["pdflatex", "bibtex", "pdflatex", "pdflatex"]
    }
  ]
}
```

- In your `.tex` file, add these lines in the top

```tex
\documentclass{article}
% Starts from here
% import the Chinese support library
\usepackage{xeCJK}
% set font for your document.
% You can find all the fonts in mac's `Font Book`
\setCJKmainfont{STSong}
```

Now, you can have a happy time coding Latex in VSCode! Enjoy!
