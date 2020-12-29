---
layout: post
title:  "[GSoC 2016] CKEditor plugins - 2. week"
author: sasanikolic
date:   2016-06-7 16:00:00 +0200
categories: gsoc-2016
keywords: drupalplanet, gsoc, drupal, ckeditor plugins, translation
permalink: /blog/gsoc-week-2/
---
Our last weekly meeting with my mentors was really inspiring. We defined new goals for the next two 
weeks and discussed various topics regarding my project. I got to know the challenges a little 
bit better and got some new motivation and an in-depth insight on what my future work will consist of. 

With the related project we realized that by focusing on some aspects, we can significantly lower the project complexity.
One of it is to make the plugins Drupal specific. This is why we agreed on the module name: *tmgmt_ckeditor*, which will contain
the CKEditor plugins that will be done for this year's GSoC project.
The second one is focusing on one specific browser. For now, we will make everything work in Google Chrome.
We believe that generalising the plugin would be a healthy follow-up if demand will arise. 

As the first version of the plugin was only an initial prototype we would like to build up on 
top of it with adding more functionalities.

## Achievements
Displaying the segments is the core functionality that I did in the first week. I worked on extending it -
now when we click on the *Display Segments* icon, we display them also in the source editor other than just
in the translate editor. This is a nice and useful feature, as the translator gets the idea how the segment 
looked before the translation. 

We now also visually anotate the current segment - currently it is colored in red. I will fix that
in the next iteration to color the opening and closing icons of the segment. Colors will be based on the 
segment's/translation status.

I opened [an issue](https://www.drupal.org/node/2742525) to discuss the segmentation semantics. 
We will use this to connect segments in the source and in the translation editor.
For now, we're using IDs. We will surely change that because we want to have the markup valid (we can't have same IDs 
on one page). 

Context interaction will be one of the key points of our plugin. When a user clicks on a word, it is displayed 
in an area below the editor. With that we also get the whole content of a segment. We can then provide 
useful translation suggestions from the translation memory based on clicked words and sentences and perform 
other actions. For that, I implemented a *CKEDITOR.event* listener for clicks and two helper functions.
One gets the clicked word and the other displays the content. The current UI can be seen in the image below.

![Displaying segments and on click actions (second version)](/assets/img/posts/second_version_plugin.png)

## Goals for next week
Our goal is to use a dummy translation of a segment and make a fully functional UI first. To achieve that, I still need 
to fix a few bugs and work on extending the code with some "fake" translation memory and other features. I will also
cleanup and restructure the code to follow an easy to read pattern that will help us in the long run.
After that, we will move to connecting it with the backend API (in plan for the week after).

I must say I am really enjoying working on this project. It is really fun, I am learning a lot 
and it feels like making a puzzle. :)


