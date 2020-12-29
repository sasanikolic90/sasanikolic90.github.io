---
layout: post
title:  "[GSoC 2016] CKEditor plugins - 5. week"
author: sasanikolic
date:   2016-06-28 20:15:00 +0200
category: gsoc-2016
thumbnail: /assets/img/google-summer-of-code.png
keywords: drupalplanet, gsoc, drupal, ckeditor plugins, translation
permalink: /blog/gsoc-week-5/
---
Week 5 is over and I have successfully passed the midterm evaluation for Google Summer of Code 2016. I really challenged myself
by choosing a project that required from me a lot of learning and constant adaptation to changes in code. As building a nice UI like Google Translate has wasn't enough for us, 
we wanted to make the [Translation Management Tool module](https://www.drupal.org/project/tmgmt) a better CAT tool by creating a completely new user interface, with a lot of new cool features, in connection with a translation memory - 
which would hold translations, their usage, quality, source, etc. 
The main goal I wanted to reach before the midterm was to build the UI that displays segments of text in the CKEditor, their suggestions from the memory and all relevant information in an area below the translation editor.
I am proud to say the progress is well visible and we are slowly but surely fulfilling all of our project requirements. 

## Achievements
This week was full of personal issues, but nonetheless I managed to meet all of my goals that I set in our last weekly meeting (described [here](http://sasanikolic90.github.io/gsoc/2016/06/21/gsoc-ckeditor-plugins-4-week.html)).

Based on a quick review by my mentor [miro_dietiker](https://www.drupal.org/u/miro_dietiker), I fixed quite a few code issues. Since last week I implemented the connection between my plugin (the UI part) with 
the [tmgmt_memory](https://www.drupal.org/sandbox/edurenye/2715815)), I got a few comments about the structure of the http request and response. I had to extend the request with source and target languages, the response on the other 
hand was lacking of more info about the source of the translation, stripped text and quality. I also changed the code to support multiple translations by adding a new level of nesting to the response with clearly describing keys and corresponding 
values. This means we can have multiple translations in the memory for one segment, which means that the user can choose between them which one he thinks is the best and use it as a translation by just a single click on a button.

Another cool feature I implemented is the listener for the changes, made by the user in the editor. I faced some trouble doing that, but with some helpful input in this [stackoverflow thread](http://stackoverflow.com/questions/37944376/ckeditor-get-changed-content-and-surrounding-html-tags) 
that I opened, I managed to do that with a timer and the [CKEditor onChange](http://docs.ckeditor.com/#!/api/CKEDITOR.event) event. This might be a bottleneck in pefromance perspective, since we are calling the http request many times because of 
the timer, but I found this to be the only viable solution for that issue for now. Once this is implemented, the addition of the data attributes for the source of the translation was easily done (if it comes from the user or from the memory).
 
The markup to do real pairing of the editors was my main focus this week. We need to support more than just two editors on the page, for example when translating multiple fields like paragraphs, we can end up having n-pairs of editors. 
To do this, a lot of refactoring was needed, which resulted in more than 300 lines of code changed. Firstly, I created a new node with paragraphs, which contain segments with specific ids. This was done in ```tmgmt_ckeditor.install```, so that it 
happens when the module is enabled - which makes the usage and the progress of the plugin available right out of the box.
After that, we needed an initial for loop over all source editors to populate the translation editors with data. This might be the only for loop in code that can exist. I removed the for loops that were present when searching for same segments and marking them as active, since 
this is very time consuming and might result in a bad performance when having many editors on page. We are pairing the editors now according to their id and name (in regex: ```*id*value-source-value$``` and ```*id*value-translation-value$```. 
This change also affected the way that the areas below the editors are displayed and how the plugin works in general. 

<figure>
    <img src="/assets/img/posts/fifth_version_plugin.png" alt="Fifth version of the plugin">
    <figcaption>With this iteration of the plugin, we got rid of one of the biggest blockers and made it (almost) fully usable.</figcaption>
</figure>

## Goals for next week
Finally, the moment has come to do a full code cleanup! This will help me significantly in future development cycles as I believe the code will be structured much better and will be easier to read. 

As for now, the only thing missing is that the segments don't cover a case with HTML tags inside. I planned in my [GSoC proposal](https://docs.google.com/document/d/1s2vqifV6rDJHXMCYKAqz7Xgd7ZEHx7zTWHVkAuwRGsg/edit?usp=sharing) to start working 
 on it this week, and so I will. The masking of HTML tags will help us to understand the structure better and cleanly show which opening/closing tags are missing inside a segment. I will firstly display an icon per defined tag (```<p>``` and ```<div>```),
 then I will create a toggle button to show and hide them.

As always, my progress and code can be found in my [Github repository](https://github.com/sasanikolic90/tmgmt_ckeditor).
