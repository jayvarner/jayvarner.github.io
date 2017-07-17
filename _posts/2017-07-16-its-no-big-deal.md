---
layout: post
title: No Big Deal
tags: [school]
date: 2017-07-16 13:00:00
authors: jayvarner
---
For the past two weeks in my UXD program, I've been working on an Lunch Money Buddy -- an app that let's parents/guardians manage paying for school lunch. We were given specs. One has really nagged at me.

> **View subsidy status:** If they participate in a school lunch subsidy program, the parent can view the status of their school lunch subsidy program account.

The first assignment was to crate [user journeys](https://www.nngroup.com/articles/customer-journey-mapping/) for four of the features listed in the specs. Because social and economic justice are dear to me, I really wanted to rock this feature. I put a lot of thought into it. I have a pretty privileged background. The extent of my knowledge of subsidized school lunch programs is that I am pretty sure they exist. I assume whatever exists is inadequate. So I looked up the [federal program](https://www.fns.usda.gov/school-meals/applying-free-and-reduced-price-school-meals).[^sad] I figured out how the program is meant to work.

I also tried to do research on designing for sensitive topics. How to show empathy in not. I did not think it would be nice constantly remind the parent or guardian that they are getting subsidized lunch. For example, on the screen showing their balance, I would not want to something like:

> You're balance is: ~~$20~~ $15 (subsidized)

In general, instead of thinking about this as a subsidy, I would rather think about it as a sliding scale - where families pay what they can and everyone eats. But that is not how things are framed or set up. So my first thought was to provide program information in the FAQ. Use notifications to alert of any paperwork deadline. The more I thought about it, and said it out loud to my partner. I began to think that people receiving the subsidy probably want a way to verify it is being applied.

In my site map, I ended up putting a "subsidy status" item in the account section. I am now even second guessing that conclusion. Maybe a more subtle version of what I initially wanted to avoid would work. Just a little `Subsidy applied` note on the balance screen.

What is clear is that I do not know which solution is best. It will be important to have an economically diverse pool of testing participants. If I have learned anything, it is that when I do not know how a feature should work, ask the people who will be using that feature. 

[^sad]:Though [the page](https://www.fns.usda.gov/school-meals/applying-free-and-reduced-price-school-meals) seems to be back, I made a gif of when I tried to look it up last week. ![Sad](https://media.giphy.com/media/l0Iy2LprMhb8EEK3e/giphy.gif)
