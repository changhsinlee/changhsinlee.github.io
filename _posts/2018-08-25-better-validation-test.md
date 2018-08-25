---
layout: post
title: Improving the Validation and Test Split
comments: true
lang: en
category: technical
header-img: "/figure/source/2018-08-25-better-validation-test/will-it-fly.png"
tags: [data-science, machine-learning, testing]
description: In this post, I am going to provide my views on the steps of train-validation-test in building a machine learning model. In particular, I'll share some thoughts on the test set, as there are more to dig into when working on real problems.
---

In this post, I am going to provide my views on the steps of train-validation-test in building a machine learning model. In particular, I'll share some thoughts on the test set. This sounds like beating a dead horse -- and in some ways it is -- but there are more to dig into when working on real problems.

## Why validate and test?

### The textbook version

When I started out, my view of testing and validation was based on textbooks such as C. M. Bishop's [Pattern Recognition and Machine Learning](https://www.amazon.com/Pattern-Recognition-Learning-Information-Statistics/dp/0387310738)

> If data is plentiful, then one approach is simply to use some of the available data to train a range of models, or a given model with a range of values for its complexity parameters, and then to compare them on independent data, sometimes called a validation set, and select the one having the best predictive performance. If the model design is iterated many times using a limited size data set, then some over-fitting to the validation data can occur and so it may be necessary to keep aside a third test set on which the performance of the selected
model is finally evaluated.

or this paragraph from [Elements of Statistical Learning](https://web.stanford.edu/~hastie/ElemStatLearn/) 2.3.2:

> An independent test set would give us a more satisfactory means for comparing the different methods.

From the books, it sounds like the goal of validation and test is to pick the best model. 

### But picking the best model for what?

Here's a tweet I saw on the other day:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">TFW you first put out a model, versus when you tune it. <a href="https://t.co/KnW9lBoSsO">pic.twitter.com/KnW9lBoSsO</a></p>&mdash; Vicki Boykis (@vboykis) <a href="https://twitter.com/vboykis/status/1028802115702009857?ref_src=twsrc%5Etfw">August 13, 2018</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

This is a situation no one working in data science would like to be in.

It was only after two years in the industry, I realized why I need to split the data for testing.

* It was not about picking the best model.
* It was not about preventing overfitting.

They are means and not the end. There's only one reason to split data and test:

**To estimate and reduce the risk in production.**

In other words, I want to get an idea of what I will see when predicting the future. If I have a time machine, then I don't need a test set -- I can just observe. But since I don't, the best I can do is using past data to _simulate_ this situation.

Here are some questions of the production issues that I may face:

1. What if I can only update the model once a month?
2. What if the model is brittle and I'm unlikely to reproduce the parameters like a deep learning model?
3. What if one of the features becomes noisy? How does that affect the performance of my model?

So many things we can test, but how?

![](/figure/source/2018-08-25-better-validation-test/will-it-fly.png)
*Will my paper model fly?*

## Splitting data

Let's walk through the model training cycle and get some idea.

### Breakdown of the training cycle

From a high level, there are three steps in training a machine learning model:

1. Train a bunch of models.
2. Pick the best model, whatever that means.
3. Get an idea what happens when the best model is deployed.

Most textbooks and tutorials say you can split the data into a 80-20 or a 60-20-20 split for different use cases. Or you can use the 80 to run a cross-validation for picking the best model, then use the 20 to test. In my opinion, this is a good starting point, but the ratio really doesn't matter.

### Validation: pick the best model

The validation set is a leveled playground for all models that will help us pick the best model based on behaviors in this playground. In this case, all the models will be evaluate on the same subset of data called a validation set, even though they may have been trained using different subsets data.

Let's say I trained 3 different models where I picked a common dataset and a single metric to optimize on. Then, to pick the best model out of 3, I can use the 3 models to produce 3 sets of predictions on the validation set. Finally, I will compare their performance on the single metric I am optimizing.

This step can and should be automated. But in all honesty, I'm still guilty of having multiple files of code that I manually compare when I build custom models.

### Test: know what to expect in the future

Now I have picked the best model using the validation set. What would happen when I roll it out?

There are a few reason that comes to my mind as to why I want to know the answer to this question.

1. Data distribution shifts over time. If I want to set up a monitoring system that sends me a note when the performance falls under certain score X. What is a good number to use for X?
2. There are metrics that everyone understands -- like revenue -- that I can estimate from a training metric like F1-score. I want to show my business partners the performance and risk of my model with simpler terms.

Here's where I found the practice of validation and test tend to become a little fuzzy. In my experience, one main reason is the following.

#### In the real world, all data is a time series data.

One common assumption in building models, at least in some academia circles, is to assume that data is stationary. In other words, assume that the distribution of underlying features for a model does not change over time.

_It is a dangerous assumption._

In PyOhio, chatting with Chris made me realize one thing. One way to classify data is to check whether how much of a "social component" it contains --

Data with low social component:
1. Things related to image, video, audio. Human perception of things rarely change over our lifespan. A dog is still a dog, and a cat is still a cat. There is good reason to assume stationary data.
2. Biological --- same reason. What makes a drug work will probably still work the day I die.

Data with high social component ---
* Sales
* Interest rates
* Election
* Crime
* Basically anything that involves human behavior (is the process of labeling cat pictures not behavioral?...hmm. Probably not.)

If you are doing image classification research, then I think there is really no reason to care about the chronological order of your train/validation/test sets.

If you work with the social component kind of data, then I think you need to be _way_ more cautious than a simple 80/20 cross-validation/test split. There should be more tests in place.

## What do I do?

So, how do I split my data for more tests? Some questions that comes to my mind are:

* Should I do a stratified split?
* If I choose a 80-20 split, should the 20 all have a later timestamp than the 80?

![](/figure/source/2018-08-25-better-validation-test/chop-it-up.png)
*Master chef in the house*

There is no one recipe that fits all validation and test scenarios. But if I keep the goal in mind -- **to estimate and reduce risk in production** -- then I can come up with sensible strategies to test my model.


### Example 1: what happens if I have less data in the future?

What's the effect of having less (or more) data in the future for my deployed model?

A strategy that comes to my mind is we can split the data in different scales.

Let's say we already set aside a test set and call Model-x the model we trained using x% of total training data. Now I can test the performance of Model-20, Model-30, ..., Model-80, etc, to test the correlation of performance and the amount of data.

### Example 2: What if I can only update my model once a month?

Maybe we can split the training and test data to a month apart?

If I have data for Month 1, 2, 3, 4, 5... and so on, then I can take the parameter get from the production model and

* train a model with data from Month 1 to predict Month 3.
* train another model with data from Month 2 to predict Month 4.

Now when I evaluate these models, I can get a sense of the staleness of a production model immediately without waiting and monitoring the model for a month in QA.

### Word of caution

I say we should be more creative in the way we test machine learning models than a 60-20-20 split. But there is a catch -- if I see the results of tests and decide this model isn't good enough, then what do I do? If I let the information of a test set to change my decision of which model to use, then it's not really an unbiased test anymore -- this is what people called "peeking" or "data leakage."

In general, I would say one should avoid this as much as possible. But the more tests you put on the model, the more likely you will see some failed tests and as a result, peek. Now we have a dilemma.

I think it is impossible to not peek in business, especially when the business keep asking for "real results." Still, the best we can do is set up the guardrails and reduce as much production risk as possible. Let's set up the right tests first and worry about whether we are peeking later.

***

By no means this is a complete guide, so please feel free to discuss in the comments or tweet at [@ChangLeeTW](https://twitter.com/ChangLeeTW).
