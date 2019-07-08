---
layout: post
title: "Becoming a Better Data Scientist: Writing Your First Test"
comments: true
lang: en
header-img: "/figure/source/2019-07-08-pytest-first-test/thumbnail.png"
tags: [data-science, tests, python]
---

If you have never written a test before, now it's time to write the first test in a few simple steps.

This post continues my previous post in the series, [Becoming a Better Data Scientist: Testing with pytest](https://changhsinlee.com/pytest-intro/)

## Flaw in my workflow

Recently, I noticed a flaw in my development workflow. Let's say I want to write data processing code in Python. Here's what I would do:

* First, I open up a Jupyter Notebook and load the data.
* Then, I try a few things to transform my data into an output `df` that looks like what I want, often with `df.head()`and `df.shape` to make sure it doesn't look wrong at a first glance.
* Afterwards, I would spot check a few things to convince myself that this code looks good, then put it into a function in a Python `.py` file.

Before I write code or modify code, I always come up with a few cases in Jupyter Notebook that I can use to check and validate. If the change I make works with the cases that I come up with, then I am confident that what I put in is a good good.

In other words, I validate my code in Jupyter Notebook to make sure that I have seen my code ran successfully, at least once, before I add the new code to the source code.

Now, I've been using into this workflow for over a year.Where is the problem?

Ummm...I was coming up with my own test cases _every single time_.

What I have noticed was that—when I need to modify code, it was easier for me to make a new example than digging through the pile of notebooks I had and recover the example I made 3 months ago.

![Let's reinvent the wheel](/figure/source/2019-07-08-pytest-first-test/wheel.png)

### Stop reinventing the wheel

As I write more code, more and more I'm coming up with new examples to check my old code. This is a classic case of reinventing the wheel. And 99% of the time, that's exactly what I get: wheels.

Even worse, reading code takes longer than writing new code. So to decipher what I was doing often takes longer than a rewrite. But that means I'm throwing away progress.

It took me an embarrassingly long to see this: no matter I'm writing exploratory or production code, if I'm coding with an example, _I might as well spend a few minutes to put that example into a test and save me some time in the future_.

If your workflow is similar to mine: you use an expected input and output to build your code, then _put that piece of data into a test._

Let me say it again:

> if you are writing code and you validate with sample data, then put the sample data into a test!

## How to write the first test

I will show you how I approach writing code these days. If you works with data and code and have not started writing tests because it seems too complicated, I believe these steps will give you a good start.

### The 3 steps to the first test

1. Write some code.
2. Validate the output.
3. Turn it into a test.

### A screencast example

At the end of the section, you can check the screencast on YouTube to see how I write my test in PyCharm.

In this example, I will use Python and the [Titanic dataset](https://www.kaggle.com/c/titanic) to demonstrate how I follow the steps.

This is what the data looks like:

![head of titanic dataset](/figure/source/2019-07-08-pytest-first-test/titanic-head.png)

#### Step 1: write some code

I wanted to calculate the average `Age` of passengers by `Sex` and `Survived`, so I wrote some code:

```py
import pandas as pd
df = pd.read_csv('titanic.csv')
output = (
    df
    .groupby(['Sex', 'Survived'], as_index=False)
    .agg({
        'Age': 'mean'
    })
)
```

#### Step 2: Validate the output

The result is stored in the variable `output` so I can show it in my Jupyter Notebook

 ![output of my function](/figure/source/2019-07-08-pytest-first-test/titanic-output.png)

Honestly, I don't know if it is calculating the numbers correctly: the numbers doesn't look _too wrong_. But the format of output is what I wanted, so it is a good time to put this test into a test case and pin the behavior down.

 #### Step 3: Turn it into a test.

<iframe width="560" height="315" src="https://www.youtube.com/embed/sySUSDVGKkA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### What did I do?

1. I wrote some code
2. I validated my code in Jupyter Notebook
3. I took the data used for validation and put them into a test case in my test suite.

When I revisit this code in the future, I can reference to the test data and won't need to reinvent the wheel anymore!

### What's next?

In this post, I only covered how to write your first test. If you have more experience with testing, you might have a few words to say on how this test can be improved. Yes, the test here can be improved, and I will cover the how-to in the future. Let us start by putting one test in so we can reap the benefits.
