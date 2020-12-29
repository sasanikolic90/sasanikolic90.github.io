---
layout: post
title:  "[GSoC 2016] CKEditor plugins - 10. week"
author: sasanikolic
date:   2016-08-02 22:11:00 +0200
categories: gsoc-2016
keywords: drupalplanet, gsoc, drupal, ckeditor plugins, translation
permalink: /blog/gsoc-week-10/
---
August is already here and the 10th week of coding for the Google Summer of Code 2016 project has come to an end. My scope for this summer is to build a functional user interface for the [Translation Management Tool module](https://www.drupal.org/project/tmgmt), 
 which will significantly help local translators in their translation process. As we are almost at the end of the project, I am starting to wrap things up in a more clean and suitable way. 
 
## Achievements
As written in my [previous blog post](http://sasanikolic.com/gsoc-2016/gsoc-ckeditor-plugins-9-week/), I struggled to find a solution for displaying the masked tags inside the segments as widgets. I believe this is a crucial part that still needs to be solved. I tried seeking 
help on stackoverflow (where I opened [a thread with my issue](http://stackoverflow.com/questions/38585145/ckeditor-nested-widget)) and I also contacted [Reinmar](http://stackoverflow.com/users/1464696/reinmar) - the maintainer of the CKEditor module for Drupal 8, but didn't get 
any response so far. I will need to spend some more time this week and solve this ```blocker```.

Other than that, my main goal for this week was to create a nice overview of the things that are currently implemented and displayed in the area below the editor, and think about the things that we can add. Because during the coding process we faced many issues and got so many 
new (crazy) ideas and added new features, which were not initially planned in the scope of my project, I needed to write a clean checklist. 

As of now, in the area below we were displaying the following:

- clicked word
- active tag
- active segment
- suggestions from the translation memory
- a button for each suggestion
- completed segments counter
- global counter of missing tags
- missing tags per active segment

Other important things, that are implemented:

- display the active segment in both editors (source and translation)
- translation memory lookup
- replacement of the segment with the suggestion
- context menu for marking segments as completed

Once I had this written down, I had to define the features, that I consider out of scope for my project. In the last weekly meeting we got a great idea of implementing a ```wizard```, that will guide the users step-by-step over the translation process. By clicking on a button, 
it would be possible to translate each segment one by one, with potential validation warnings in between the translation process. I believe this is absolutely out of scope, as it would take too much time for now and we have other priorities. 
Another thing that I won't find time to implement is an external library of definitions and synonyms of words (a ```dictionary```). This needs to be done in TMGMT and we'll have to consider this once the initial version is complete.

After that, my next goal was to create a mockup of my ideas, how the final product should look like. As a reference point, I took Google Translate, which has a really nice and useful UI, but with so many functionalities, it can really be confusing sometimes. My initial idea was 
to separate the UI into three main parts. The area inside the editor, the area below the editor and a new one, a scrollable sticky division at the top right corner.

The area inside the editor consists of the widgets for the masked tags and the segments. The tags should be draggable with a handle bar, while the segments should persist in their position. While the segments are defined as block elements, the tags are inline elements. 
This means that both, tags and segments are nicely marked and easy to spot in the editor's area.

For the are below the editor, I need to adapt to the Drupal 8 GUI. As everything seems to be quite squared, with straight lines and borders, my proposal would be to have line, stating which segment is being translated and a simple table, where the rows would be the 
translation suggestions and columns would hold the following: 

- Quality (of the translation suggestion)
- Source (human or machine)
- Translation suggestion
- Use suggestion (a button)

<figure>
    <img src="/assets/img/posts/area_below_mockup.png" alt="The mockup of the area below the editor.">
    <figcaption>The mockup of the area below the editor.</figcaption>
</figure>

For the quality, I used the [HTML5 meter element](https://css-tricks.com/html5-meter-element/). It represents a scalar measurement within a known range, or a fractional value. The attributes are really easy to set and I like the look of it.

If you ask me, the buttons in Drupal 8 are not that user friendly, especially if used in a context seen above. I would change the add suggestion button to a button with a check icon and see how that looks.

The ```sticky division``` would hold some important information, such as the counter of the completed segments and the validation counters. I proposed to this two things here, because they are both numerical counters and are easy to spot at first sight. The background of this 
element would adapt to the tag validation. If all translations would be valid, it would be green, in other case I would set the background to red. In our weekly meeting with my mentors, we discussed this matter. They didn't like the solution for the validation part and would rather 
have it in the area below the editor. The reason for this is understandable, as the area below the editor holds the parts, that the user can interact with - and the validation part that displays the missing tags is clickable, so that the user can place them in the editor at the cursor position. 
I will need to rethink and redefine my mockup to fit the validation part properly in the following week.

I already implemented my ideas on a separate branch called [mockup](https://github.com/sasanikolic90/tmgmt_ckeditor/tree/mockup), which will still be in constant development until we find a proper final solution.

## Goals for next week
From this weekly meeting with my mentors I got some inputs on how to test and improve my code performance. I will work on that for a day or two, but mainly I will work on the mockup and other small fixes that we found lately. 
I will also implement the adding of the missing  tags from the validation message, since this is not implemented yet - I needed some feedback there.
