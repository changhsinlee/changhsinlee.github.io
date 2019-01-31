---
layout: post
title: "Agile in Data Science: What is Waterfall and Agile?"
comments: true
lang: en
header-img: "/figure/source/2019-01-30-agile-waterfall/digging.png"
tags: [data-science, agile, methodology]
---

There has been a lot of talk about Agile around me recently. For that, I want to summarize my understanding of Agile in data science into a series. This post covers where the discussion usually starts: Waterfall versus Agile.

Agile is a software development methodology that has no lack of reference from

*	[Wikipedia ](https://en.wikipedia.org/wiki/Agile_software_development)
* [Agile Manifesto](https://agilemanifesto.org/principles.html)
* and many many many other blog posts.

Therefore, this 3-part series is my own take on answering three questions:

* What is Agile?
* Does Agile work in data science?
* If the answer is yes, how does it help me?

But before I go into Agile, let's talk about Waterfall, a term that always got brought up when someone says Agile.

## What is Waterfall?

The study and the chart that associates with Waterfall can be first traced back to the paper by Dr. Winston Royce in 1970.

* [Managing the Development of Large Software Systems](http://www-scf.usc.edu/~csci201/lectures/Lecture11/royce1970.pdf)

 Waterfall got the name because it describes a product development process that looks like a cascading waterfall. Here's a figure I took from Royce's paper:

![](/figure/source/2019-01-30-agile-waterfall/royce.png)

Each stage in the development is handled by a different team. Once a stage is complete, the responsibility of product moves to the next team.

In the context of software engineering,

* The business team comes up with the specification of the whole software.
* The developer team builds the whole product based on specifications passed down.
* The QA/Testing team tests the product the developers put out.
* After it passes the testing phase, the product is then shipped out of the door.

### But Waterfall is not a methodology

In fact, in the paper, Winston Royce described Waterfall as **what not to do.** This is the sentence he put under the chart:

> I believe in this concept, but the implementation described above is risky and invites failure.

The case against Waterfall is that people claim this methodology leads to delay and cost overruns. Royce noted that, if any change is needed in the testing stage,

> In effect the development process has returned to the origin and one can expect up to a 100-percent overrun in schedule and/or costs.

Despite the fact that it has a name, Waterfall _is not a methodology_. I think it's better to call it a _process_. In the field of research,

**Waterfall was never presented as a good method to use in developing software.**

...until the US Department of Defense happened: it was put into [DOD-STD-2167](https://en.wikipedia.org/wiki/DOD-STD-2167), the Department of Defense's specification for all military software, in 1985. [It was a mistake.](http://beza1e1.tuxen.de/waterfall.html) From [Craig Larman's research](http://www.craiglarman.com/wiki/downloads/misc/history-of-iterative-larman-and-basili-ieee-computer.pdf), he said

> Ironically, in a conversation nearly a decade later, the principal creator of DoD-Std-2167 expressed regret for creating the strict waterfall-based standard...

So Waterfall was never meant to be.

## What is Agile

Let's look into Agile. I am not going to list out the points from Agile Manifesto, but rather, provide my views on what led the authors to summarize the best practices they saw into a manifesto.

### What does Agile try to solve?

I believe that the goal for any software project should be

**shipping the right product on time.**

If Waterfall leads to overruns, then Agile tries to prevent anything that lead to an unshippable product:

* a product that does not match business need
* huge delays in projects
* low quality software that breaks often.

The next question is, how do I practice Agile? My view of Agile is simple:

### Agile is Waterfall compressed

Unlike building a house, where having one completed bathroom doesn't do much,

* software can provide value with only basic features.

There can be 10 features in a product, but they don't provide the same amount of value to the customer.

Instead of designing, building, and testing all 10 features in each stage, why not stick to a few features that makes up a usable product, then run a Waterfall on it?

In particular, what if we limit the Waterfall process to only one feature? By limiting the scope,

* business can communicate their need faster, and the team can ship the high priority feature to users first
* the overrun cost to start over is the cost of one or a few feature instead of a large chunk of the project.

### Hey, this sounds like common sense

It's like solving a math problem—it's easy when you _see_ the solution.

I agree it sounds like common sense, but I didn't know about it—in fact, it took me a year in business to learn. Let explain why.

### Shipping on time

Why does Agile work? There are two ways to ship the product on time.

1. Build faster.
2. Make better estimate on when will I ship.

Agile encourages shipping a minimal viable product (MVP) with a few features, so it takes less development time than a full product. This means I can build faster.

For me, it also helps on 2 by fighting a problem that I didn't see when I first started in building software.

### Combating optimism

Fredrick Brooks said it right in his seminal work, The Mythical Man-Month:

> All programmers are optimists.

I used to believe that once I complete my code, then _it just works._ Through time, I learned that the truth is the opposite—there is always a bug, or rather, a way to break my code that I didn't think of.

Besides my natural tendency to assume that a task will take only the minimal required time to finish, I think that humans are generally terrible at making any estimate for a big project. How many public construction projects completed on budget and on time?

The good news is, unlike construction, I have the privilege in building software to create business value by shipping a single feature.

This means that understanding Agile helps me fight my optimism: each time I deliver a feature, I learn a little bit more about the project. Now I have additional data to make a better estimate on the next feature, and the project is more likely to get out of the door on time.

This is what I think Agile is—it's not about writing code, but about managing the complexity outside of code. In an ideal world, both Waterfall and Agile will produce the same code. So what is the best framework for me to deal with the "other stuff" that blocks me from the success of project?

![digging-through](/figure/source/2019-01-30-agile-waterfall/digging.png)


## Summary

* Waterfall was never presented as a good software development methodology. _Don't do it._
* Agile is compressed Waterfall on features. The goal is to ship software on time.

### Next up: Agile in data science

There are two pressing questions:

#### 1. Does it apply to data science?

Agile is about building software, and I am skeptical about applying, say, all of the [Agile Manifesto](https://agilemanifesto.org/principles.html) to data science.

I will explain the reasoning in the next post, but my thought revolves around one of my belief: measure of quality for data science products can be _different_ from software products.

When I optimize on a different goal, the process should also be different.

#### 2. There are two popular implementations of Agile:

* SCRUM
* Kanban

which one of them works better for data science, or none of them? From what I heard among data science teams, Kanban seems to be the winner. I will also explain what they are in the next post.

***

I'd love to hear your stories. If you have a love/hate experience to share, please leave a comment below ⬇️ or tweet at [@ChangLeeTW](https://twitter.com/ChangLeeTW). Thanks for reading!
