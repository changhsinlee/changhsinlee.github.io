---
layout: page
title: "Configuring the Business with YAML—PyderPuffGirls Episode 6"
comments: false
lang: en
# header-img: "/figure/source/2019-01-07-pyderpuffgirls-ep5/earplug.png"
tags: [data-science, python, sql]
permalink: /ep6-draft/
---

This post is a follow up on the last about another tool for meaningful abstractions in code—the configuration file. In particular, I will show you a config file format, YAML, and how it works in Python.

## Introduction

In the last post, I showed how to use Jinja2 to generate SQL queries from business logic parameters. What if I have multiple files that can use the same parameters? What if I have a project wide setting that I want to be able to change only once when needed?

This is where _configuration files_ come in.

### Example of config files

Although I didn't notice this pattern until recently, config files are everywhere. They are a way for users and developers to change the behavior of software without getting into the granular detail of the code.

They also come in many different formats. Here are some examples. Please take a quick read and see if you can get an idea what those files are trying to do:

* [The config for my blog](https://github.com/changhsinlee/changhsinlee.github.io/blob/master/_config.yml) uses YAML.
* [The config for Airflow, a popular workflow engine for scheduling batch data jobs](https://github.com/puckel/docker-airflow/blob/master/config/airflow.cfg), uses TOML.
* [A config for Counter-strike: Global Offensive, a popular video game](https://gist.github.com/nickbudi/3916475), uses a custom format.

### Why config?

In the examples

* My blog is linked to my social network accounts by simply providing the handle.
* Airflow uses config to decide where the job logs should be, the address for web UI, how often does the scheduler refreshes, etc.
* Games use configs to let players tune the control settings. The "Settings" in games are often graphical representations of the underlying config files.

In all 3 examples, I may not understand the actual code, or the code could be proprietary like Counter-Strike. But as a user, it does not matter—

**I can change the behavior of software without knowing the code.**

In other words, it is a _convenient abstraction_. The business was abstracted away from the code. The config files, when used right, improve user experience and reduce cost of maintenance—if I decide to link a different Twitter account for my blog, all I need to change is one place.

The same idea can be applied to data analytics workflow. But how to implement depends on the context. I will show you a particular format, YAML, that you can start playing with.

## A common format: YAML

Here's a yaml file. It contains key-value pairs just like Python dictionaries.

```yaml
protein:
    - pork
    - chicken
    - beef

vegetables: carrots, cucumbers

fruits: [apple, banana]

christmas: 2018-12-25

numbers:
    - 123
    - 456

colors:
    apple: red
    banana: yellow

words: {one: uno, two: dos}
```


When I read it into Python,

```py
import yaml
with open('d:/pyderpuffgirls/ep6/sample.yaml') as file:
    data = yaml.load(file)
```

this is what `data` looks like

![yaml data](data.png)

and I can plug the values in the dictionary to my code—[like what I did in the last post](https://changhsinlee.com/pyderpuffgirls-ep5/#the-python-code). That's it!


One thing to be careful with YAML is that it has a list of reserved keywords—for example, `yes` becomes `True` and `no` becomes `False`. The full YAML syntax is also a can of worms, so I will not attempt to cover it. [This is a good reference](https://camel.readthedocs.io/en/latest/yamlref.html#overall-structure-and-design) that should satisfy most of the data analytics needs, and I stick to only the simple use cases.

### A YAML example

Let's say I have a baseball business report that has to go out every Sunday and pulls data from all American League teams and aggregate by their division. In my configuration file, I might have

```yaml
report_day: Sunday

AL_East:
    - Yankees
    - Red Sox
    - Rays
    ...

AL_West:
    - Mariners
    - Athletics
    - Angels
    - Rangers
```

In 2013, Major League moved the Houston Astros into Americal League West to make the division 5 teams.This report now has to include the Astros. This is a business decision that I have no control of.

Changing the business logic in a complex report workflow is probably the most underappreciated job in analytics. Even more so when people think it's a small change in business, and therefore "can't you _just_ change this one thing and give me the report now?"

The good new is, if I had set up my code with a configuration file that captures the correct business logic, then all I needed to do is change one place—in my example, I will add "Astros" to AL_West and that should be it. There is no need to modify the code.

## Learn more abstractions

Business is complex, and the complexity rubs off on analytics. This complexity in business has to live somewhere in the code—I can choose to let it live inside a single file, or I can abstract it away into a separate file from my implementation code.

It is up to me to decide whether to add this layer of abstraction or not. If I decide to use a config file, then I am adding at least one more file to the project, so the structural complexity goes up. But maybe the overall code complexity comes down. Everything has a trade-off. It boils down to one question: "does the overall cost of the project go up or down?"

The answer clearly depends on the context. To me, the real benefit of learning new abstractions is that _abstractions give me options_. Just a year ago, I wasn't aware of config files. Learning to use it has changed how I view code. Today, I have the options to those questions.

## What's next?

In the next post, I want to show you how to run command line tools in Python. In particular, I will show you how to use the module `subprocess` on your own machine, and maybe later `paramiko`—how to run commands and manipulate files on a remote server.
