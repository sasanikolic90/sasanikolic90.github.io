---
layout: post
title:  "[GSoC 2016] CKEditor plugins - 1. week"
author: sasanikolic
date:   2016-05-31 13:05:00 +0200
categories: gsoc-2016
keywords: drupalplanet, gsoc, drupal, ckeditor plugins, translation
permalink: /blog/gsoc-week-1/
---
The Google Summer of Code 2016 coding period started last week, 
May 23rd and since then, every student started to actively work on their project.
After the one-month long community bonding period, it was time to start coding.

I had many things scheduled for this first week. Firstly, I found some interesting 
mockups to get a broader idea, what we are building and how it should look like. 
Secondly, I created a [GitHub repository](https://github.com/sasanikolic90/tmgmt_ckeditor) 
and a [sandbox project](https://www.drupal.org/sandbox/sasanikolic/2737249) on d.o.

After that preparation, I immediately jumped into it and went through some tutorials on how to make custom 
[CKEditor](http://ckeditor.com/) plugins for Drupal. I found [this tutorial](https://medium.com/@CayugaSoft/creation-of-extended-functionality-for-the-basic-ckeditor-in-drupal-8-f9e6990aa48f#.19v3bp7b4) 
really useful. It was really easy to follow and it guided me step by step. I have 
also read many discussions about custom plugin development and how to convert them to Drupal.
And there is also some nice documentation on the [CKEditor API](https://www.drupal.org/developing/api/8/ckeditor) 
for Drupal.

A general overview of the CKEditor and it's development was also presented at DrupalCon Amsterdam 2014 by [wwalc](https://www.drupal.org/u/wwalc).
<iframe width="420" height="315" src="https://www.youtube.com/v/h9KV_VRvIG8" frameborder="0" allowfullscreen></iframe>

## Achievements
With all those nice guides and examples, it was fairly easy to make the first version of my plugin.
I created a new Drupal module, found some nice icons and started working with the javascript file.
The initial version is using user-defined ```<tmgmt-segment>``` tags in the source code. With them we 
define specific segments (e.g. divisions and paragraphs), that are then easier to translate.
As by our goals, when the user clicks on the *'Display segments'* button, a simple search is performed on 
our tags. They are then replaced by some nice icons, showing where the segments start and end. This enables 
the translator to see them clearly and perform his actions.
This is shown below.

![Displaying segments (first version)](/assets/img/posts/first_version_plugin.png)

## Goals for next week
For next week we want to extend the functionality of the initial plugin by displaying 
the context below the text area and show the segments in both editors (source and translation) - to clearly 
distinguish the currently marked segment.
Also, a specific word that was clicked should be displayed below. We can then use it to gather 
possible translations, check if it already exists in the TMGMT memory, etc.
I also believe the module needs to be renamed to something more simple and easier to read - 
on our weekly meeting we agreed on the name *tmgmt_ckeditor*, since we are building a TMGMT 
specific CKEditor plugin.

You can find my code on [GitHub](https://github.com/sasanikolic90/tmgmt_ckeditor) 
and follow my progress there. Looking forward to start implementing some more fun stuff!


