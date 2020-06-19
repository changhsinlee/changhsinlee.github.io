---
layout: post
title: "Import custom .py module in Google Colab from GitHub URLs"
comments: true
lang: en
type: note
header-img: "/figure/source/thumbnails/pen-paper.png"
---

How to import custom `.py` modules from places like GitHub.

## An example

In our meetup, we were studying this blog post and trying to reproduce this in [Google Colab](https://colab.research.google.com/):

* http://www.degeneratestate.org/posts/2018/Mar/24/causal-inference-with-python-part-1-potential-outcomes/

In the first code block, there was a line `import datagenerators` that was causing import errors. It turned out that it was [a custom module hosted on GitHub](https://raw.githubusercontent.com/ijmbarr/notes-on-causal-inference/master/datagenerators.py). How can we import this module?

## The code

It turned out we can download and write the file to Colab's working directory and import from there:

First, make sure `requests` is installed. In the first cell, run

```
!pip install requests
```

Then in the next code cell,

```py
import requests
# Save datagenerators as file to colab working directory
url = 'https://raw.githubusercontent.com/ijmbarr/notes-on-causal-inference/master/datagenerators.py'
r = requests.get(url)

# make sure your filename is the same as how you want to import 
with open('datagenerators.py', 'w') as f:
    f.write(r.text)

# now we can import
import datagenerators as dg
```

That's it!