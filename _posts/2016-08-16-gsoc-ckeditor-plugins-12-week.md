---
layout: post
title:  "[GSoC 2016] CKEditor plugins - 12. week"
author: sasanikolic
date:   2016-08-16 19:36:00 +0200
categories: gsoc-2016
keywords: drupalplanet, gsoc, drupal, ckeditor plugins, translation
permalink: /blog/gsoc-week-12/
---
This week marks the official end of my Google Summer of Code 2016 project. For the past 3 months I have been working hard on developing a rich and usable user experience for translators using the [Translation Management Tool module](https://www.drupal.org/project/tmgmt) in Drupal 8.
The main goal was to create two CKEditor plugins. The first plugin would convert text parts into segments to easily _segment the content_ into smaller bits and enable easier translation management. It's also connected with the [Translation Memory](https://www.drupal.org/sandbox/edurenye/2715815), 
to provide saved translation suggestions for requested segments. The second plugin is about _masking HTML tags_ inside segments. This really helps to understand the structure better and cleanly show which opening/closing tags are missing inside a segment.

I believe the goal was clearly reached quite some time ago. After that, we started implementing more functionality around these two main "pillars". Users can now click through segments, which are clearly shown in both, the source and the translation editors, and check in the area
below the editor for the translation suggestions. They can be used to replace the selected segment. Each segment can contain HTML tags, which are masked in the pre-process, before populating the content. They are displayed as ```widgets```, as they can be clickable and draggable.
If the translation is missing some tags, a warning message is displayed for awareness. When the translation seems to be fine, it can be marked as completed. This increases the counter of completed segments in the sticky division in the top right corner and colors the segment with a green background.

All of the above currently works fine and more extra features will be added soon!

## Achievements
I started last week as always, by working on the feedback that I got from my mentors on our weekly meeting. This mainly involved some UI fixes, like writing a cleaner validation text and displaying the missing tags not as just links, but with the same styling as the tags that are in the editor.
I've also added a title for these missing tags, which is displayed when the user hovers the mouse pointer over them. This helps the user to understand that those are connected and when clicked, they can be placed at the cursor position inside the editor. Also to to save space for smaller screens 
and to tidy up the table with the translation suggestions in the area below the editor, we're displaying just a more simple text inside the button - "Use" instead of "Use suggestion". Another UI feature that I added on my mentor's suggestion was to mark the active segment that has any 
missing tags with the red color to indicate it more strongly and clearly.
 
Then I moved to the most important thing, the code cleanup, which I think was really needed. I was already using [ESLint](http://eslint.org/) - a pluggable JavaScript linter, as a PhpStorm plugin extension, during the coding process which helped me a lot with the code quality, but I felt there 
could be done something more to run everything faster and cleaner. 

That is why I met with one friend of mine during the weekend and we spent some time reviewing hwo the plugins work and figuring out what could be done better.
There were three main things that needed to be done:

- Split the bigger Javascript functions into many smaller ones when possible
- Check how often these functions are called and adjust their scope properly (if they are not called often, we can put them inside the scope of their parent function)
- Check for loops for better performance

I have also added function comments based on the [JavaScript API documentation](https://www.drupal.org/node/2183405) for Drupal and checked everything for the correct syntax (variable names, data types, tag orders...). I am sure the code is much more easier to read now for other developers in the community. 

While working on this, I found out some problems regarding JavaScript closure inside loops. This is related to the binding of of click events for adding the missing tags in the editor. This was an intriguing challenge, but [Stack Overflow](http://stackoverflow.com/questions/8909652/adding-click-event-listeners-in-loop)
 was (as always) a really helpful source for finding a good working solution.
 
I also spent quite some time trying to solve the issue regarding widget dragging inside the segments, which are defined as block elements, but still didn't find any proper solution. I also tried to contact [nod_](https://www.drupal.org/u/nod_) and [Wim Leers](https://www.drupal.org/u/wim-leers), but 
got no positive feedback. Any help from the community on that matter would be appreciated. :)

Finally, I would like to make a quick overview of how to test my module, list the things that are working and things that are still missing and need work.

How to test it:

- make sure you have all the dependencies (Translation Management Tool, Paragraphs, Translation Memory)
- install the module
- for now, only the predefined nodes from my module are working - a simple ```Test for CKEditor plugins``` and ```Test for CKEditor plugins with paragraphs```
- request a (german) translation of either one of those two
- enable plugins on translate/review page in the editor toolbar
- test it (and possibly send me some feedback)

What works:

- dummy nodes for testing with new text format
- works on nodes with one or more editor pairs (paragraphs)
- masking and unmasking the HTML tags
- displaying the active segment in both editors
- translation memory querying
- displaying suggested translations from the memory
- using suggested translations - placing the selected one in the active editor
- validation of missing tags (globally and per segment)
- adding of missing tag
- set segments as completed
- masked tag dragging

What is currently not working:

- text segmentation (in tmgmt_memory)
- saving segmented content properly, so that accepting translation does not save segments but initial content
- responsiveness

Note, some of the "not-working" issues listed above are not on my side; either they are out of scope or from a related project. 

Another important thing that we should be aware of: this project provides a number of puzzle pieces towards providing translators and reviewers better tools to work with TMGMT. Many other things are needed until this fully works together with real content.

My latest code can be found in my [Github repository](https://github.com/sasanikolic90/tmgmt_ckeditor) and in my [sandbox project](https://www.drupal.org/sandbox/sasanikolic/2737249).

## Goals for next week
For this last week, I am planning to wrap up my project completely, update the documentation if needed and create issues for my sandbox.

## Conclusion
I am very thankful to my mentors [Miro Dietiker](https://www.drupal.org/u/miro_dietiker) and [Sascha Grossenbacher](https://www.drupal.org/u/berdir) for the constant supervision, guidance and support throughout this amazing experience. Our future plans involve making the initiated project
fully functional in real world with true translations and real usage. When we will think that it is ready, it will be merged into the TMGMT module and I will actively maintain it. 

I would also like to thank Google for this great opportunity and the open-source community for the support with my project and for pushing technologies forward step by step, every day. During this summer I gained many technical and psychological skills. 
On one side I got to know myself better and while working remotely I gained a lot of self-discipline and motivation, on the other side it was a real challenge to study and learn JavaScript, together with the CKEditor API. I am sure all the skills gained will pay back someday!
