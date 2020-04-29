---
layout: post
title: "Manage multiple Python versions on Windows with py.exe"
comments: true
lang: en
type: note
header-img: "/figure/source/thumbnails/pen-paper.png"
---

`py.exe` is a useful tool on Windows that I didn't know.

On Windows, `python.exe` defaults to the latest version installed, but sometimes I want to use older versions, say, 3.6 for debugging, even if I have 3.8 on my computer. 

`py.exe` is the command line tool that comes with Python installation that solves the version management issue.

## How to use

I recommend running `py -h` to see what you can do with it.

The command `py -0` lists all Python installations available.

![List all python installations](/figure/source/notes/2020-04-28-windows-py-launcher/py0.png)

Even though Python 3.8 is my default Python, R=running `py -3.7` launches the 3.7 shell:

![Launches 3.7 shell](/figure/source/notes/2020-04-28-windows-py-launcher/py37.png)

## When to use

I mostly use it to generate virtual environments for different code repositories pinned at a specific Python version. For example,

```sh
py -3.8 -m venv .venv
``` 

creates a virtual environment `.venv` with Python 3.8

## Note on installation

Having `py.exe` means you can simply download and install as many version of Python from the [official Python website](https://www.python.org/downloads/) as you want.

During the installation, there is no need to check either of the boxes to make use of the py launcher as checking the first one requires local admin rights which may be impossible for corporate laptops.

![No need to install for all users](/figure/source/notes/2020-04-28-windows-py-launcher/both-unchecked.png)