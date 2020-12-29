---
layout: post
title:  "[GSoC 2016] CKEditor plugins - 11. week"
author: sasanikolic
date:   2016-08-09 20:10:00 +0200
category: gsoc-2016
thumbnail: /assets/img/google-summer-of-code.png
keywords: drupalplanet, gsoc, drupal, ckeditor plugins, translation
permalink: /blog/gsoc-week-11/
---
We've come to the last phase of coding for my Google Summer of Code 2016 project, and things are currently going quite well. The scope of developing CKEditor plugins for the [Translation Management Tool module](https://www.drupal.org/project/tmgmt) was really fascinating so far,
with many challenges to solve and new things to learn every day. I believe my work will significantly help local translators in their translation process by providing a nice content segmentation and translation suggestions. The idea is to create a tool similar to Google Translate, 
 but with some extra features, like segmenting the text and masking HTML tags inside them.

## Achievements
[Last week](http://sasanikolic.com/gsoc-2016/gsoc-ckeditor-plugins-10-week/) I discussed about the complete overview list of all the things that are already implemented and how I imagined the UI for the new translation process. 

As I got quite some feedback from my mentors on the mockup that I created, I have focused on this part this week. I have extended it with definitions and synonyms for a specific (selected) word that we get for the marked word from the [Translation Memory](https://www.drupal.org/sandbox/edurenye/2715815) and 
created some screenshots, that can be found in my [Google Drive folder](https://drive.google.com/open?id=0B3PPGWVax5aya29IWmRZNENxVlk). Any tips and ideas about the UI are strongly appreciated.

While working on this topic, I realised that the current scale for the quality of the translations in the memory is not perfectly suitable, since it ranges from 0 to 5. I suggested to the maintainer of the Translation memory project to extend the quality scale to 10. I believe 
this would result in a more accurate scale of marking the translation's quality and would be easier for the translator to decide, which translation is better. For that, I opened [this issue](https://www.drupal.org/node/2781253) on drupal.org. With the usage of real quality values for 
the translations, we can get rid of all the hardcoded testing values. The source values are yet to be discussed and implemented properly, after that I can merge the ```mockup``` branch, which contains the new UI with the main ```master``` branch. The code for the mockup with all 
the updates can be found [here](https://github.com/sasanikolic90/tmgmt_ckeditor/tree/mockup).

Another topic that we discussed on our previous weekly meeting was about the validation area. My first idea was to put it in the ```sticky division``` on the right top corner, but with some thinking, we agreed to put it back in the area below the editor. I changed the visual 
appearance of the warning and proposed two solutions - one that follows the Drupal guidelines for general error messages on the page and another one, that is more simplistic.
 
Apart from the UI, I had to fix different issues that I spotted along the testing process. I finally managed to fix the widgets issue described [in this blog post](http://sasanikolic.com/gsoc-2016/gsoc-ckeditor-plugins-9-week/). I did that by setting the ```tmgmt-segment``` element 
as a ```div``` element and the ```tmgmt-tag``` as an inline widget. I believe the nesting issue was caused by setting the ```tmgmt-segment``` tag to contain text with this line of code: 

``` html
dtd['tmgmt-segment'] = {'#': 1};
```

The next problem that I'm having is with dragging the widget inside the segment. I can't figure out why the parent segment is created and displayed around the tag when the dragging happens. I will have to dig deeper in the dtd and widgets topic.

I also spent some time on the performance part, solving an important issue regarding the call of the ```onChange``` event function. I am using the ```debouncer``` to limit the invocations of this function in a given time frame, since before that, if the user was typing some text 
the function was called many times, resulting in significant resource consumption.

<figure>
    <img src="/assets/img/posts/full_word_mockup.png" alt="The full word mockup.">
    <figcaption>The image shows the current status of the mockup for the word selection.</figcaption>
</figure>

## Goals for next week
My workload for next week will be very intense, since I have so many issues opened on various topics.

Mockup/UI:

- Use the shorter text for buttons (just "Use")
- Remove the "Use suggestion" in the table header
- Display the active segment red if there are missing tags
- Change the validation text and the display of the missing tags in the area below
- Ordering of the translations should be ascending, not descending, based on quality value

Code cleanup:

- Add/remove comments
- Extend the documentation
- Find bottlenecks
- Optimise the code for better performance
- Test with bigger nodes, which have many segments and tags

Fixes:

- The bug with dragging widgets

Other:

- Update the sandbox project
- Import translations from a file
- Support real translation jobs by applying the segmentation automatically on the source text
