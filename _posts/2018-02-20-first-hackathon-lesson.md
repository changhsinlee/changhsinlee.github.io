---
layout: post
title: My First Hackathon
comments: true
lang: en
category: meta
header-img: "/figure/source/2018-02-20-first-hackathon-lesson/charge-counts.png"
tags: [data science, R, hackathon]
description: My experience and a lesson from going to my first Hackathon.
---

Last November, I went to my first hackathon, the Queen City Hackathon, in Charlotte. Here's my review on the event with a lesson I learned from it.

## Before the Hackathon

I heard about the hackathon in September. It was a hackathon with a data science focus. I wanted to go, but didn't want to team up with strangers, so I signed up with another data scientist from my team, Brad. Neither of us had been to an event like this, and we had little planning before the event.

When I signed up for the hackathon, my only goal in mind was to learn new tools. If we can win the cash prize, then great. But winning was unlikely without a plan. I told myself that I will use a tool that I have never touched before, so I could use this opportunity to improve my technical skills.


## Day 1

Brad and I got to the venue at 6 PM on Friday. I was surprised to see how many people were in the venue, and how many of them brought sleeping bags. We didn't bring any. After a brief introduction to the rules and the datasets, we sat down at a long table in the middle of the room. Two other participants, Mahua and Noelline, asked if they could join us. We said yes, why not, and started working together.

There were six datasets for us to choose from, and the first thing we did was picking a theme. We decided to use 2 of the 6 datasets --- Charlotte Police Arrest Records and 311 Service Requests --- and tell a story about the neighborhoods in Charlotte. Because we had never seen the data before, we also decided to explore two different directions at the same time. Mahua and Noelline dived into criminal charges, while Brad and I work on an exploratory map. Our thought was that, by plotting the arrests and 311 requests together on a map, maybe we could eyeball the correlations between the two datasets.

Making a maps sounded easy, but we had never worked with geographical data. It turned out that arrest records only had street names but not numbers, while 311 requests had only latitudes and longitudes. It took us 4 hours to clean the data and aggregate the data by zip code. When Brad and I made our first map, it was past midnight. We called it a day.

![number of criminal charges in Charlotte](/figure/source/2018-02-20-first-hackathon-lesson/charge-counts.png)
**Number of charges in Charlotte by zip code, 2014-2016**

## Day 2, the pitch

We came back at 7:30 next morning. Mahua and Noelline didn't sleep and spent the night making a report on what they learned from the charges. We have 4 hours before the initial pitch to judges, so we made a final tweak on the maps and the reports, then spent the rest of morning working on a slide deck for our pitch. By noon, we were exhausted. We pitched, but the judges didn't select us as finalists.

Brad and I stayed for the final pitch. It turned out that most of the finalists built web apps. There were apps on insurance cost, traffic alerts, and so on. People loved them. I'd love to have some of those apps on my phone if they were actual products on the market. I saw several great use cases of D3, Flask, and different JavaScript frameworks in an hour.

We didn't win, but it was a fun 24 hours. I started thinking about what I would do if we come back next year.

## Lesson - apps are better for the pitch

I thought about why we didn't make the finalists. We made reports and maps to satisfy our own curiosity about Charlotte neighborhoods, but we didn't put much thought into the pitch. When we made the slides, the communication was a one-way street, and the audience probably felt that they were listening to a story from a local new channel.

On the other hand, most finalists made apps. They also presented their products in a way that made me feel that I would be the one in control. The audience was way more engaged when they saw an app.

I've seen a similar pattern at work with internal clients. Some of our team projects put out interactive apps --- even in the earliest stage of the project --- that let our clients play with the data they are familiar with. If the clients liked the product or the idea of the project, then they came back with comments and requests, and they became early investors in the project.

The same pattern applies to hackathons. In business, reports and insights can be more valuable than apps, but _a good hackathon pitch is a good startup pitch_ --- they draw investments. There's a good reason why most tech startups make apps and not reports.

I didn't know how to build apps, but the hackathon showed me some great tools with great examples. I'll be more prepared next time.
