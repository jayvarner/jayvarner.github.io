---
layout: post
title: Search is Hard
tags: [school]
date: 2016-09-11 13:30:00
author: jayvarner
---
Search is a tricky user experience to design. Advanced search is even harder. If you need advanced search, are you doing it wrong?

Trying to build an advanced search interface is actually what pushed me into a masters program in user experience design and what is now being funded by a grant from the Knight Foundation. I found it interesting that, in this week's presentation, the instructor used his experience testing an advanced search interface. To a fault, I might have paid more attention to the narrative and less to the instruction. Nonetheless, the example hit home. For a good year and a half, I have been banging my head on an interface to search thousands of geospatial assets that are confined to a rather small geographic area. I'm confident in my work to return results based on what metadata we have. I am even good with returning results that are weighted based on keywords. I feel positive about my work to return results weighted by coverage of a geographic index[^dork]. I can return quality results all day long. It means absolutely nothing if people do not realize how to generate those results.

So, if you need advanced search, are you doing it wrong? Yes, yes you are. Researchers go to an advanced search option when they are dissatisfied with their initial search results. Why would you bother designing an experience for a person who is always-already frustrated? There is no way to win. That was fun to say and is totally aspirational.

[^dork]: That barely means anything to me. If you are interested, I'm talking about old scanned maps. Say you live really want to see the maps that include your neighborhood. However, your neighborhood might be a outside the historic city limits. An unweighted search might tell you that there are hundreds of maps that include your neighborhood. Your excitement quickly turns to disappointment as you realize that cartographers historically used your neighborhood as a space for a legend. The results should be weighted based on how close to the center the search point. I also have some prototype functionality to differentiate patterns that look like geographic features vs metadata features.
