---
layout: post
title:  "[GSoC 2016] CKEditor plugins - 6. week"
author: sasanikolic
date:   2016-07-05 16:45:00 +0200
categories: gsoc-2016
keywords: drupalplanet, gsoc, drupal, ckeditor plugins, translation
permalink: /blog/gsoc-week-6/
---
Another week passed by of my Google Summer of Code 2016 project. My [goal](https://docs.google.com/document/d/1s2vqifV6rDJHXMCYKAqz7Xgd7ZEHx7zTWHVkAuwRGsg/edit?usp=sharing) for this summer is to create a revamped 
user interface for the module called Translation Management Tool, with many new features that will simplify the translation process for both, end-users and translators. 
I divided my goals into two parts:

* creating segments out of the content, mark them in the editor and perform actions based on the clicked word and segment
* masking HTML tags inside segments

As we already passed the midterm evaluation, half of the project is already done in the previous weeks. I moved to the second part of my project this week. More details about my work is described below.

## Achievements
As for every week, I started my weekly development cycle by refactoring the code based on my mentor's comments. Since [last week](http://sasanikolic.com/gsoc/gsoc-ckeditor-plugins-5-week/) I fixed an important blocker and added the support for multiple editors on the translation and review pages,
we agreed that the editors should only work in pairs. We now clearly mark the same segments in editor pairs and toggle the CKEditor plugin buttons accordingly. I did this by implementing a new [JavaScript Object Prototype](http://www.w3schools.com/js/js_object_prototypes.asp), which contains 
many relevant information that we want to store about the editor pair, such as:

* ID of the editor pair
* the selected editor name
* the DOM element of the area below the editor (in which we display selected segments, words and suggestions)
* the selected word
* the selected segment's id
* the counter of segments, that are marked as completed

Other than refactoring and fixing smaller issues, I started working on the new plugin for displaying the masked HTML tags inside the segments. The purpose of masking the tags is to help us understand the translating text's structure better and cleanly show which opening/closing 
tags are missing inside a segment. Firstly, I extended the dummy translatable node in the ```tmgmt_ckeditor.install``` file 
with a masked ```<b>``` tag that looks like this: 

```html

<tmgmt-tag tag=”&lt;b&gt;”>text inside a masked tag</tmgmt-tag tag="&lt;/b&gt;">

```

Once I had a tag to work on, I created a simple plugin that just displays some arrows instead of the opening and closing of the masked tags. It is fully dependent on the plugin that is displaying the 
 segments, which means it is only enabled and available to toggle when the segments are shown, and disabled otherwise. 
 
For the next steps, we should discuss about the attributes of the masked tags. 
We should for sure preserve them throughout the translation process. In the example of alt and title tags that would possibly need translation, we could have them hidden and display them on mouse hover, so 
that the user is aware of them being present.

<figure>
    <img src="/assets/img/posts/first_version_tags_plugin_2.png" alt="First version of masked tags">
    <figcaption>First version of the masked tags plugin.</figcaption>
</figure>

## Goals for next week
The plan for next week is to continue on developing a valid definition of tags and extending it's functionality by handling data attributes properly.

Feel free to check my [Github repository](https://github.com/sasanikolic90/tmgmt_ckeditor/) for being updated on the constant progress during my weeks.
