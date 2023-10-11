---
title: "Passing the Azure AZ-302 Exam"
summary: My tips & tricks for the AZ-302 exam for those of you wanting to transition from the 70-535.
date: 2019-03-14
draft: false
categories: ["Exams"]
tags: ["Azure", "Exams", "Learning"]
thumbnail: "/img/thumbs/exam-scrabble-thumb.jpg"
url: "/exams/passing-the-azure-az-302-exam"
---

{{< rawhtml >}}

<img src="/img/cert-badges/azsaexp.png" width="30%" align="right">

{{< /rawhtml >}}

Firstly apologies for the radio silence on my blog, it’s been a hectic couple of months for me as I’m getting married in April; time free at the weekends is very sparse at the moment. But fear not I have a list of items to blog about and a new method to attack writing them faster, so watch this space.

Anyway on with the topic for today’s blog!

So nearly a month ago now I passed the Azure AZ-302 exam to complete the requirement for the Azure Certified Solutions Architect Expert badge.

Now it was my 2nd attempt that I passed on, but failing exams is not something to be ashamed of at all. Putting yourself up against a test is a sign of courage and confidence in my eyes. We can only learn from failure; so that’s a massive positive in my eyes! It took me a while to realise this in my career after failing a CCNP exam by 3 points years ago!

Before I continue I feel it is important to reiterate a couple of points that I made in my post about passing the [70-535.](/exams/passing-the-azure-70-535-exam/)

**You can only take the AZ-302 if you have passed the 70-535 exam. Also the transition exam, AZ-302, is only available to take until the 30th June 2019 before it is retired!**

## Preparation – What To Study

As I have said in my previous post, you must first understand what the exam is going to be testing you on before you can decide your plan of attack for studying the exam objectives.

As always Microsoft has published the exam objectives and breakdown on the [AZ-302 web page.](https://www.microsoft.com/en-us/learning/exam-az-302.aspx)

![AZ-302 Exam Skills Measured](/img/az-302-skills.png)

However please note that a [change document](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE2MRod) was posted on the web page that changes some of the exam objectives and section percentages; it’s located just above the exam objectives as a note.

One thing I feel is important to say about the stated exam objectives for the AZ-302 is that they look at first glance to be quite a wide range of topics and it can feel overwhelming.

From taking the exam twice I honestly feel that the exam objectives aren’t very accurate at all. So much so that I actually filled out parts of the feedback at the end of my 2nd exam.

So my advice would be to review the objectives on the web page and in the change document and make notes of things you need to revise more heavily than others. But then take the following tips from my experiences:

- You don’t need to know how to be a full on developer for this exam. An understanding of programming languages and being able to read them at a high level will suffice.
  - It’s more important to understand what service is best for each use case for different development functions (i.e. Azure Functions/Azure Web Jobs etc…)
- Know your SLAs for different services and features (Availability Sets 99.95% vs Availability Zones 99.99% etc…)
- Invest more time on Azure Site Recovery & the different Azure Backup products/features
- Be prepared to be quizzed on some preview features, so don’t disregard those sections of the Azure Docs

## Preparation – Revision Tools & Resources

I find videos are easier to watch on the train when travelling into London and back home as I can’t always get my laptop out to make notes; getting a seat is generally a challenge!

So with that in mind I started off with the [Scott Duffy AZ-302 course on Udemy.](https://www.udemy.com/az302-azure/)

This course is very high level and doesn’t have a lot of walkthroughs/labs/demos so it’s certainly not going to be enough for you to pass the exam. But it’s quite good for the theory side of things around determining requirements and when to use a pilot instead of a POC.

Then I normally like to read a book to reinforce the videos but as this exam is only a transition exam there isn’t one available and I can’t really see anyone thinking about making one as it’s such a niche gap in the market.

So instead I headed to the [Azure Docs!](https://docs.microsoft.com/en-gb/azure/) Especially from my last experience with the 70-535, these really are awesome for all aspects of learning Azure. From how tos to overviews, they cover it all and are super up-to-date thanks to them being open the community to recommend updates via the github repo where the content is hosted.

I also make sure I check the [Azure Blog](https://azure.microsoft.com/en-gb/blog/) daily at least twice. Once when I get up and again before I call it a day. This helps to keep on top of new feature releases etc…

I cannot stress enough of also using the Azure portal itself daily and just getting comfortable with it and it’s behaviour. Also any hands on time with AZ CLI would be a very handy.

Finally I checked all of the following great blogs from some people in the community to make sure I had covered all the correct objectives and pick up any tips they have given in their posts:

- [Gregor Suttie](https://gregorsuttie.com/2019/01/10/az-302-exam-study-notes/)
- [Thomas Thornton](https://thomasthornton.cloud/2019/01/21/microsoft-azure-exam-az-302-study-notes/)

## Preparation – Method

My method to revise for this exam was very similar to how I approached the 70-535 apart from I spent much more time actually deploying and configuring things in the Azure portal and with AZ CLI.

A high level overview of my approach per objective/topic is below:

- Watch any videos available for the objective/topic
- Deploy it and configure it if possible and understand how it really works in Azure
- Read at least the overview page on the Azure Docs website for the objective/topic but also try to follow some of the how tos through
- Make notes along the way of key facts like SLAs, tiers & pricing etc…

## The Exam

The exam itself was a new one on me for Microsoft exams with the introduction of hands-on labs!

Yes that’s right, actual labs where you have to perform tasks in the Azure portal and using AZ CLI/PowerShell. I had 2 labs that contained about 10 tasks each! And being perfectly honest, I really enjoyed them as i use the portal daily testing features & services out so it didn’t phase me.

I can only assume that these are marked by Microsoft reviewing the JSON of the subscription you are given access to to perform the tasks on and they look for the specific values you have been asked to configure/change. Very cool!

There was also the normal multiple choice questions along with a couple of case studies with 5 questions or so each in them.

A top tip of mine would be to make sure you don’t waste all your time in the labs as my case study questions appeared after I completed the labs; just when I was starting to put my feet up!

## Summary

All in all I really enjoyed the exam; enough to take it twice in fact!

The introduction of labs was odd for an architecture exam but I’m a big believer of being able to actually configure/deploy something before suggesting it in a HLD as how can you know it really the works they way it says it does!

Hopefully the above gives you an insight to my experiences for this exam and helps you pass it before the retirement date in June.

Until next time… 