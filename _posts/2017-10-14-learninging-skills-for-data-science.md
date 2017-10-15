---
layout: post
title: How I Learned Data Science Skills as a Math PhD
comments: true
---

This post is a review on how I picked up data science skills when I was a PhD student in mathematics. The time span was from January 2016  to May 2017, before I got my job. I break it into three periods: before, during, and after my summer internship. Here, I will talk about what I did, and not what I think I should have done --- that's for another post.

## The Transition to Industry

Three and half years into graduate school, I wanted to leave academia more than ever. In December 2015, I decided to look into my employment options seriously. I had no idea, however, on how to start this transition into the industry. People said math skills are valuable, but the context was never about PhDs. I didn't see why a company would hire me. The recent graduates from our department went for actuary or software engineering jobs, but I didn't know how they made it. So, I started my first job search in the United States.

I looked into three areas: management consulting, software engineering, and data analytics. First thing I noticed was that companies love STEM majors, as long as it's not *pure* Math. They wanted Comp-Sci, Physics, Biology, Economics, and all sorts of engineering degrees, but no one wants pure Math. "Strong analytical skill" by itself meant nothing. The reality was that I didn't have a marketable skill set. It seemed to me the most concrete skills I could put on my resume was programming.

But I knew only one language back then: MATLAB. It was for homework on partial differential equations. Plus, there were close to zero MATLAB jobs. I had to learn another language, but what could I learn in less than a year?

![analysis-paralysis](/figure/source/2017-10-14-learning-skills-for-data-science/analysis-paralysis.png)

I was lost. I turned to another student of my advisor, Jiayi Jiang, for help. Jiayi was six months away from graduation but already went through several job interviews. He told me that my best career bet was going into software engineering or machine learning. I asked for more detail about the machine learning route. He then told me that all the interview questions he got were within the scope of a single book --- [Elements of Statistical Learning](https://web.stanford.edu/~hastie/ElemStatLearn/) (ESL), which might be a good start for me. So ESL became my entry point of data science.

## The Skills
### Before the Internship: R and SQL (Jan-May 2016)

I quit ESL by chapter 2. It was a great read, but it was all math and no programming. My real need now was *learning to program*. After more research, I found that the authors of ESL had put out another book called [An Introduction to Statistical Learning](http://www-bcf.usc.edu/~gareth/ISL/) (ISLR) with R exercises. I started going through the chapters with all the R exercises, and it was wonderful. The math was simpler than ESL, but I got to learn R.

At the same time, I started applying to internships. Something called SQL was required on every single one of the job postings, so I looked into how to learn SQL by myself. I bought the kindle version of the book [Head First SQL](http://shop.oreilly.com/product/9780596526849.do) based on Reddit recommendations. (Warning: the kindle version was horrible, don't buy it.) The learning process was dry and painful. I had to force myself through the chapters because the value of SQL wasn't clear to me. I stopped after I finished select and where clauses, right before the chapter on joins.

I spent 30 minutes a day to learn besides writing my resume and applying for jobs. In late February, I got my first response when I was at a math conference in Maryland. The email contained a few questions that I answered next day. The day after, they wanted me to take a data challenge. I almost told them I wasn't ready for that. Instead, I told myself to give it a shot. I managed to scramble together a data report with the help of ISLR, Google, and caffeine.

The offer came in mid-March. Now that I had an internship lined up, I decided to pause my plan on learning so I can get as much dissertation research done as possible before the internship starts in May.

### During the Internship: R, SQL, and Statistics (May-Aug 2016)

On my first day as an intern, I read a Tufte book. I was waiting for a work laptop from IT, and my boss asked: "Have you read Tufte?" and put a copy of [The Visual Display of Quantitative Information](https://www.amazon.com/Visual-Display-Quantitative-Information/dp/1930824130) on my desk. He was cleaning the team library that day. I had never heard of Tufte, but it was a great read and an eye-opening exposition to data visualization for me.

![tufte](/figure/source/2017-10-14-learning-skills-for-data-science/mr-tufte.png)

Over the next two months, this sort of exchange happened again and again. Many of my discussions pointed me to a resource in statistics that I had never heard of. The terms that my colleague used became little gold conceptual nuggets that I could plug into the search bar to expand the comfort zone of my knowledge. It was a lot of fun.

The analytics team I was in was an R and SQL stack. I spent 5-6 hours a day on R and SQL for my project. My coding skills improved. But the real value came from the help and code review I got from my mentors --- they gave me a better idea on which learning goals to set and how to achieve them. As a graduate student, I was isolated in the world of mathematics research. It was through the internship that I re-discovered the importance of peers in the learning process.

### After the internship: Python, R (Aug 2016-May 2017)

After I came back from the internship, I got to teach the statistics lab in the Fall. After two months of work, I wanted to show my students why R was awesome for data analysis. To prepare my [lecture notes](https://github.com/changhsinlee/RLabNotes), I read several books like [R for Data Science](http://r4ds.had.co.nz/) and [Applied Predictive Modeling](http://appliedpredictivemodeling.com/). My students also came to me with all kinds of questions, project ideas, and bugs.

Besides R, I started learning Python. Back in March, I tried to get into Python with the tutorial [Learning Python the Hard Way](https://learnpythonthehardway.org/). It was so boring that in two weeks I stopped and never went back. The isolation and uselessness of material killed my desire to learn. With that unpleasant experience, I changed my approach this time. I talked my friend Armenak into learning Python with me using another book that I found on Reddit, [Automate the Boring Stuff with Python](https://automatetheboringstuff.com/). It turned out to be the best intro-level programming book I had ever used. Armenak and I met at the Starbucks twice a week, where we followed the instructions from the book closely. I learned to make all kinds of throwaway scripts, including my first web scraper. It was fun and useful. The ability to write simple Python scripts had been a great asset at my current job.

The internship experience made me realize that the best way for me to learn was getting constant feedback on things that I did. My students gave me feedback in R, and my friend gave me feedback in Python. It worked.

![peer-feedback](/figure/source/2017-10-14-learning-skills-for-data-science/feedback.png)

### So, is that it?

No, I learned more than R and Python after the summer internship, but I can't go into detail without first talking about [meetups](https://www.meetup.com/). Looking back, I was lucky to find two awesome Nashville communities: [Developer Launchpad](https://www.meetup.com/Developer-Launchpad-Nashville/) and [Penny University](https://groups.google.com/forum/#!forum/penny-university), that helped me build, filter, and prioritize the list of things to learn at a faster rate than anything I had before. I will explain why meetups are awesome in the next post.
