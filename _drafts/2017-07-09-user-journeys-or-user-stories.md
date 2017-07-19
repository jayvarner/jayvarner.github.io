---
layout: post
title: User Journeys or User Stories?
tags: [school]
date: 2017-07-09 13:00:00
authors: jayvarner
---
For years, I have been working in an agile setting. The product owners write user stories, I deliver the story, and the product owner accepts or rejects the story.

User stories, on a micro scale, describe a testable action and are written in the format of

```As a <role>, I want <goal/desire> so that <benefit>``` [^userstory]

When written correctly, a user story describes the smallest, testable functionality. For example:

```As the Dude - I want my rug back - because it really tied the room together.```

In my UXD master's program, I spent the last week working with `user journeys`.

I've been going back-and-forth while comparing the two. It comes down to a classic macro vs. micro debate. As a developer, I'm comfortable with user stories. However, writing user stories is a skill. User stories often do not describe the whole story, or parts of the the story are "implied."

For example, I recently had a story

```As a user, I want to create an account so I can save my work.```

This story has one obvious implied story, the ability to log in. Furthermore, there is no mention of things like:

- Does a user have to confirm their email address?
- Can a user reset a forgotten password?
- Can someone use Google, Facebook, etc. for authentication?

If we had created a user journey for this, I think we would have thought through all these interactions and had a much better idea of the feature's scope. If my colleagues are game, I'm going to suggest we experiment with writing user journeys for our next project.

[^userstory]:[https://en.wikipedia.org/wiki/User_story#Format](https://en.wikipedia.org/wiki/User_story#Format)
