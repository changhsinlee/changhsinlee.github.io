---
layout: post
title: "A Python Tutorial on Automating Boring Data Workflow—The PyderPuffGirls"
comments: true
lang: en
header-img: "/figure/source/2019-02-22-pyderpuffgirls-s1-guide/thumbnail.png"
tags: [data-science, python]
---

Back in November, I started an internal training for my colleagues on walking through basic automation in Python for data analysis. This post is a guideline on how to use the training material.

I started this tutorial because I believe that most Python tutorials are about writing applications and not scripts. But I write more scripts than applications at my job—how can I automate away my data analysis workflow with scripts?

This post has two parts:

1. A guide to the PyderPuffGirls posts
2. How to solve the most common Windows Python setup issues

I might make another post on how to use the command line on Windows if we decide to start a round two training.

## The PyderPuffGirls

**Disclaimer** Too many people have asked me this question so let me clarify where this name came from: when we started, we wanted to give this series a name. After a heated discussion, my colleague Keith Finch came up with the winner. I took and ran with it 😂. As a prize, Keith gets a permanent spot in the post:

![Keith](/figure/source/2019-02-22-pyderpuffgirls-s1-guide/keith-buildings.png)
*Keith with the Python Staff of Enlightenment†*

PyderPuffGirls has a total of 8 episodes that form 3 groups.

### Group 1: Automating Excel reports

This group will take you from getting data in a database to an Excel spreadsheet for your users. If this looks like part of your job:

* Run SQL queries
* Download data to PC
* Turn the data into an Excel spreadsheet

then you might find the following posts helpful.


* [Episode 1: A Python Tutorial for the Bored Me](https://changhsinlee.com/pyderpuffgirls-ep1/) - why I wrote this series
* [Episode 2: How to Query a Database in Python](https://changhsinlee.com/pyderpuffgirls-ep2/) - how to run queries
* [Episode 7: Replace the Home Tab in Excel with Python](https://changhsinlee.com/pyderpuffgirls-ep7/) - how to make Excel reports
* [Episode 8: Making Excel Charts, Formulas, and Tables with Python](https://changhsinlee.com/pyderpuffgirls-ep8/)

Note: I am planning to update Episode 7 later with a clip in Excel on formatting strings based on Jared's suggestion.

### Group 2: Getting rid of two clicks

These two posts aim to remove two mouse clicks: when to start, and how to give the report to others via email.

In other words, this two posts turn your workflow into an asset you can run it while you sleep:

* [Episode 3: Don't Wait, Schedule and Relax Instead](https://changhsinlee.com/pyderpuffgirls-ep3/) - how to use `scheduler`
* [Episode 4: Filling Up Your Inbox with Goodies](https://changhsinlee.com/pyderpuffgirls-ep4/) - how to send email via SMTP

### Group 3: Reducing SQL headaches

These two posts deal with the problem of simplifying a long and repetitive SQL query that leads to mistakes and errors.

* [Episode 5: Untangle the SQL Mess with Jinja](https://changhsinlee.com/pyderpuffgirls-ep5/) - templating the queries
* [Episode 6: Make a Workflow Config with YAML](https://changhsinlee.com/pyderpuffgirls-ep6/) - metaprogramming

## Appendix: Troubleshooting Windows

There are a few things for Python users on Windows that will take a while to figure out for the first time. I am putting the most common issues here.

### Where do I install Python?

When installing Python, I recommend installing in the `AppData` folder. For example, on my laptop, I put in

```
C:\Users\chang\AppData\Local\Programs\Python\Python37\
```

The reason is that `C:/Program Files`, the default in Python installation, requires local admin rights—which can be a big headache if you work a corporate job. Putting it in `AppData\Local` means you don't have to worry about this.

### Python/Pip is not recognized

A common problem on Windows is that running `python` or `pip` in a command line gives

```sh
python is not recognized as an internal or external command
```

or

```sh
pip is not recognized as an internal or external command
```

The problem is that, when you type a command into Windows command line (cmd, PowerShell, Git Bash, etc), it will look scan all the folders defined in the "PATH variable" to find the corresponding executable (`.exe` files). But `python` or `pip` is not added to PATH variable by default. Therefore, what I need to do is to add them into my PATH variable.

**Steps for Windows 10**:

1. Type "path" in the search bar
2. Choose "Edit environment variable for your account"

![searching-path](/figure/source/2019-02-22-pyderpuffgirls-s1-guide/path-search.png)

3. In user variable, select PATH and click Edit
4. Select "New" and put the Python base folder and the `Scripts` folder in there.

For example, if I installed Python in `C:\Users\chang\AppData\Local\Programs\Python\Python37\`, then I can check if I do have a `python.exe` in there:

![python-folder](/figure/source/2019-02-22-pyderpuffgirls-s1-guide/python-folder.png)

In this case, the two folders I need to add into my PATH are

* `C:\Users\chang\AppData\Local\Programs\Python\Python37\`, and
* `C:\Users\chang\AppData\Local\Programs\Python\Python37\Scripts`

![python-path](/figure/source/2019-02-22-pyderpuffgirls-s1-guide/path.png)

**Steps for Windows 7**:

The steps for Windows 7 is almost identical to Windows 10, except that paths in PATH variable is concatenated into a single string separated by a semicolon `;`. I usually copy the string to Notepad++, edit it, then paste it back in.

Note: `Scripts` is the equivalent of `bin` folder for Unix-based systems.

[Here's a link to relevant StackOverflow answer](https://stackoverflow.com/questions/23708898/pip-is-not-recognized-as-an-internal-or-external-command)

### File path in Windows

Windows and Unix use different delimiter for the file paths:

* Windows uses backslashes e.g. `C:\Program Files`
* Unix uses forward slashes e.g. `/usr/bin`

In Python, backslashes are reserved for [escaping characters](https://python-reference.readthedocs.io/en/latest/docs/str/escapes.html), that is, to tell Python that anything that follows a backslash is used with a special purpose.

This creates a problem for Windows file paths and many `FileNotFound` errors. Let's say I want to read in a file called `C:\test stuff\words.txt`, what do I put in `file_path`?

```py
file_path = ???

with open(file_path) as file:
    text = file.readlines(file_path)

```

There are several options:

#### 1. Use two backslashes

```py
file_path = 'C:\\test stuff\\words.txt'
```

#### 2. Use raw string

A string with an `r` prefix is a raw string. This means anything is read as-is and nothing is escaped—so backslash does nothing.

```py
file_path = r'C:\test stuff\words.txt'
```

#### 3. Use forward slash

Most of the time, I can simply change all the backslashes to forward slash. Python will understand what is going on and pick the correct file

```py
file_path = 'C:/test stuff/words.txt'
```

#### Use more pathlib

In using Python, I prefer using the `Path` object from `pathlib` for better cross-platform compatibility. This is, however, out of scope. Therefore, I'll give you a post if you want to learn more about it:

* Trey Hunner: [Why you should be using pathlib](https://treyhunner.com/2018/12/why-you-should-be-using-pathlib/)

### Additional tools in the toolkit

I found that there are many tools that can help you automate the workflow, especially if you have a Windows machine. In particular,

* Git
* Cmder


#### Git

To install Git, go to the official website.

* [Link to Git homepage](https://git-scm.com/)

When you install, make sure you choose "Add Path" so you get the full benefit of the Unix tools such as `cd`, `nano`, etc. Otherwise, you can add the `bin` folder into the PATH variable for the same effect.

#### Cmder

The native Windows command line tool `cmd` is cumbersome, and PoweShell uses its own syntaxes that can be confusing to first-time users. I prefer using third-party command line tools that imitates an Unix command line like Git Bash or Cmder.

Git Bash is a good alternative to cmd _but unfortunately_ [Git bash can't run python](https://stackoverflow.com/a/32599341). On most Windows computers, running `python some_script.py` in Git Bash leads to a frozen command line.

So I prefer `Cmder`

* [Link to Cmder homepage](https://cmder.net/)

which a Terminal simulator is much easier to navigate than `cmd.exe`.

### Acknowledgement

Thanks to Brennan Hodge, Jared Cline, Caroline Collins, Rebecca Johnson, David Crane, Chris Ruesch, David Pirie and Keith Finch—the PyderPuffGirls—for joining me on this amazing trip!

† The Python Staff of Enlightenment is a courtesy of David Gilman ([@dgilman](https://twitter.com/dgilman)), co-organizer of [the Charlotte Python Meetup](https://www.meetup.com/python-charlotte/).

---

Back in Episode 1, [a redditor asked me to provide email subscription](https://www.reddit.com/r/datascience/comments/a0oj2a/a_python_tutorial_for_the_bored_analysts/eakia92/), so I finally set it up. If you like this series and want to keep up with other series and posts, then you might want to consider subscribing to my mailing list.

<!-- Begin Mailchimp Signup Form -->
<link href="//cdn-images.mailchimp.com/embedcode/slim-10_7.css" rel="stylesheet" type="text/css">
<style type="text/css">
	#mc_embed_signup{background:#fff; clear:left; font:14px Helvetica,Arial,sans-serif; }
	/* Add your own Mailchimp form style overrides in your site stylesheet or in this style block.
	   We recommend moving this block and the preceding CSS link to the HEAD of your HTML file. */
</style>
<div id="mc_embed_signup">
<form action="https://changhsinlee.us20.list-manage.com/subscribe/post?u=3f0aeffb807b292a987cb206d&amp;id=397cacef3e" method="post" id="mc-embedded-subscribe-form" name="mc-embedded-subscribe-form" class="validate" target="_blank" novalidate>
    <div id="mc_embed_signup_scroll">
	<label for="mce-EMAIL">Subscribe to my newsletter</label>
	<input type="email" value="" name="EMAIL" class="email" id="mce-EMAIL" placeholder="email address" required>
    <!-- real people should not fill this in and expect good things - do not remove this or risk form bot signups-->
    <div style="position: absolute; left: -5000px;" aria-hidden="true"><input type="text" name="b_3f0aeffb807b292a987cb206d_397cacef3e" tabindex="-1" value=""></div>
    <div class="clear"><input type="submit" value="Subscribe" name="subscribe" id="mc-embedded-subscribe" class="button"></div>
    </div>
</form>
</div>

<!--End mc_embed_signup-->

Please leave a comment and help me make it better!
