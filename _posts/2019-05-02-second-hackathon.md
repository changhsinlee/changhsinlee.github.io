---
layout: post
title: "Winning my Second Hackathon"
comments: true
lang: en
header-img: "/figure/source/2019-05-02-second-hackathon/thumbnail.png"
tags: [data-science, design-thinking]
---

On March 24, 2019, I won HackathonCLT. Here are my reflections.

![Our team holding the check](/figure/source/2019-05-02-second-hackathon/midnight.jpg)
*Team Welch Labs: Me, Kurt, and Stephen at 2:30 AM*

## Hackathon

In 2017, I went to my first hackathon, Queen City Hackathon, and had a glimpse on how hackathon works. I went in for learning, and I put the lessons I learned in a previous post in February 2018, [My First Hackathon](https://changhsinlee.com/first-hackathon-lesson/). This time, I teamed up with Stephen Welch and Kurt Dowswell—and we won. So I'll share what I learned this time as well.

PS. all participants had to wipe the data and code before leaving the hackathon, so I won't be able to show you what we built.

## A traveling checklist

Before I go to the hackathon, I decided to write a checklist on what to bring and shared with my teammates. This was what I had, word for word from Google Docs:

```
# Things you will regret tier
Power strip - MUST BRING
Deodorant
Things to sleep on
Clothes (socks etc - you might sweat a lot because there’s a lot of people in one room)
Laptop + cables
Mouse/touchpad, if you like using a mouse
Dongles for Apple stuff
Sleep mask + Earplugs so you can actually sleep if needed

# Helpful tier
A monitor - especially if you are used to dual or triple monitor setup.
Extra water - last year Queen City Hackathon ran out of bottle water
Clickers for presentation
Gums
Toothbrushes, toothpaste, floss or any travel basics - gotta protect my teeth 😂
A luggage to put everything in there

# Chang’s excessive preparation stuff:
A folding table - in case we can’t secure a good spot
Two folding chairs
```

If I were to start over, I will still bring

* Extension cord and power strips
* Chairs and tables
* A monitor

Finding an outlet was impossible. Take a look at the power system near our station at 5 AM:

<iframe width="560" height="315" src="https://www.youtube.com/embed/1PodLoyXkrA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

This outlet could very well have been overloaded...

There also wasn't enough chairs at the venue. The two folding chairs I brought turned out to be a huge boost to productivity. I wouldn't be able to handle an all-nighter sitting on the floor.

All the items that I brought for sleeping turned out to be useless—we didn't sleep at all. There were plenty of Red Bulls but coffee ran out very early. If I were to participate again, I would bring a water bottle so I could refill at the fountain instead of walking across the venue to get a few sips.

## Design thinking helped us win

We were chatting and getting dinner at the venue, and Kurt mentioned that he went to a UX/UI meetup for a design thinking workshop a few weeks ago.

When he mentioned the word **design thinking**, I was like _hell yeahhhhh_.

I first learned about design thinking from Code on The Beach last August. One of the talk I went to was Jon Bosworth's ([@jbosworth](https://twitter.com/jbosworth)) talk on how they used design thinking to plan the conference.

Here's the talk: Validating A Product Before Writing A Line Of Code: A Five Day Design Sprint in 60 Minutes.
<iframe width="560" height="315" src="https://www.youtube.com/embed/QtthmhAd4og" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

It was my first contact with design thinking—and I had been a huge fan of the process since.

For us, we wanted to use try it because we wanted to generate lots of ideas in a short period and validate the usability before getting down to code.

We wanted to design a good solution, but there was a big constraint: we only had about 12 hours to come up with a working solution. How much time should we spend on design? We decided to go slow and spend at least 1-2 hours to work out a plan before we started coding.

### The Crazy Eight

To generate ideas, Kurt proposed that we use an exercise from the design thinking called the Crazy Eight.

* https://designsprintkit.withgoogle.com/methodology/phase3-sketch/crazy-eights

The exercise was simple: you take a blank sheet of paper, split it into 8 sections, and make 8 sketches on ideas about the topic at hand _in 4 minutes_. 30 seconds per sketch. It was _hard_.

<iframe width="560" height="315" src="https://www.youtube.com/embed/L30noD9o9Ns" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Here are my eights:

![my crazy eight](/figure/source/2019-05-02-second-hackathon/crazyeight.jpg)

Drawing the ideas on paper led to another 30 minutes-plus of discussion on what we could build. Our final plan emerged at around 11:00 PM. The whole design took us almost 3 hours. But it was a great plan and a story that we could all stand behind.

## My thoughts

In no particular order.

### A game plan before the event

We have never worked together before, so, during the week before the event, we discussed about what kind of tools each of us were comfortable with and what we can do with it. We also exchanged ideas on what we wanted to build.

None of our ideas was viable for the actual hackathon. For example, we thought about hosting our app on the cloud, but the sensitivity of healthcare data meant we couldn't host the data anywhere outside of our laptop.

Still, having a premade team meant we got a lot of the coordination out of the way before the event started. In my first hackathon, this step took well over an hour. Getting rid of the communication overhead meant that we could get down to design and code immediately.

### Focus on action instead of analysis or model

The dataset we had was from the hospitals and all of them were aggregated. There simply wasn't much we can do in terms of modeling without domain knowledge on these datasets. No machine learning.

Instead, we focused on answering the question: what are the actions that basic insights from can bring? We decided to compare the data to CDC's public data to highlight the public health issues in each clinic's service area so the medical professionals can communicate with patients via text based on this information.

This pattern was similar to what I saw in data science in business: we don't need complex model to turn data into action.

### Integrate early

We planned to integrate our code early. At 2:30 AM, I had to take a walk after I finished my part of code. I was optimistic that we could get it done in an hour or so at that point:

<iframe width="560" height="315" src="https://www.youtube.com/embed/pUK4FgnsBfQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

We ended up spending over 3 hours on integration and working on the pitch. We had a great story, but the critical path of the app didn't work until 5:30 AM, 30 minutes before the submission deadline.

I don't know if we could actually finish the product and the pitch earlier than we did, but I think the mentality of focusing on shipping the first product out helped.

### It's still about learning

What makes hackathon worth it? I still think that it was all about learning. What's the best way to learn? Find great teammates with a diverse range of skill sets.

In less than 12 hours,

* I learned to make GIS maps in Python
* I learned to send text to my phone via Twilio
* From my teammates, I learned how Kurt design and build a web app, how Stephen perfects the visualization and tells a story, and how they tackle the problems.
* And I put an exercise from design thinking to use.

And then we won. It was an amazing night.

![holding the check](/figure/source/2019-05-02-second-hackathon/trophy.jpg)
