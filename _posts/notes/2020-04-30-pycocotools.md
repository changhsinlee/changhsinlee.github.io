---
layout: post
title: "Guide to installing Pycocotools/CocoAPI"
comments: true
lang: en
type: note
header-img: "/figure/source/thumbnails/pen-paper.png"
---

`pycocotools` is a dependency in `detectron2` that is difficult to install.

* https://github.com/cocodataset/cocoapi

I ran into a lot of trouble getting it to work when I was installing [Facebook's detectron2](https://github.com/facebookresearch/detectron2), so I'm going to jot down a few things I tried.

## 1. On Windows

If you are on Windows, don't even try to pip install from the original repo. [They do not support it](https://github.com/cocodataset/cocoapi/issues/9#issuecomment-217339740).

### Pip solution: use a fork

Instead, use this fork

* https://github.com/philferriere/cocoapi

Note that you must have Visual C++ Build Tools to compile the files which may require local admin rights. The repo's README has a thorough installation guide.

### Conda solution

I don't use `conda`, but if you do, this reply in the issue may help you:

* https://github.com/cocodataset/cocoapi/issues/169#issuecomment-462528628

## 2. If you get `numpy` or `cython` not installed

The `pycocotools` repository does not maintain a `setup.py` file and you may run into dependency issues.

Here's a fork that proposed a fix:

* https://pypi.org/project/pycocotools-fix/

Or you can manually install the dependencies yourself. This is why `detectron2` says `pip install cython` before installing `pycocotools`.

## 3. VPN issues

When I'm on company VPN, I got this message doing `pip install` from the origina repo

```
curl: (56) Malformed encoding found in chunked-encoding
```

This is during the process of `git clone` before it does `pip install`.

My guess was that the company VPN is modifying the data stream coming from GitHub in some way and the `cocoapi` repository may have some large binary files that caused the issue. 

I ended up downloaded the whole repository as a zip file and unzip it and use the unzipped version as part of a Docker build.

![](/figure/source/notes/2020-04-30-pycocotools/zip.png)



