---
layout: post
title: "Run multiple Python versions on Windows with py launcher"
comments: true
lang: en
type: note
header-img: "/figure/source/notes-default/thumbnail.png"
---

`py.exe` is a useful Python tool for Windows that I didn't know.

## How to use

This is a command line tool installed under `C:/Windows` that comes with the Windows Python installation that helps you pick which Python version you want to run.

## When to use

A common place that I use it is to generate virtual environments for different code repositories for a specific Python version. For example,

```sh
py -3.8 -m venv .venv
``` 