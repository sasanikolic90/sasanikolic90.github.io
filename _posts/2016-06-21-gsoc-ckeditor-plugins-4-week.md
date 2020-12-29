---
layout: post
title:  "[GSoC 2016] CKEditor plugins - 4. week"
author: sasanikolic
date:   2016-06-21 21:16:00 +0200
categories: gsoc-2016
keywords: drupalplanet, gsoc, drupal, ckeditor plugins, translation
permalink: /blog/gsoc-week-4/
---
The 4th week of coding for Google Summer of Code is over and the evaluation period is starting. 
My goal for the first half of the coding process was to create a functional CKEditor plugin for displaying segments 
in connection with [tmgmt_memory](https://www.drupal.org/sandbox/edurenye/2715815). My main part is the UI with specific focus 
on segments semantics and functionalities, the memory, on the other hand, contains suggested translations of segments, their 
 quality and other useful information. Together with my mentors, we want to make TMGMT as pleasant as possible for the end users (translators).
I believe my work is currently progressing really well and I think I've met most of our expectations so far. You can check my progress [here](https://github.com/sasanikolic90/tmgmt_ckeditor).

## Achievements
First of all, I wanted to make the usage of my plugin easier and right out of the box. That is why, I've updated the ```tmgmt_ckeditor.install``` file to preload
a new text format (```translation_html```) and a dummy translatable node with some dummy segments. This is all 
loaded when the module is enabled. I've also added new dependencies for the ```tmgmt_demo``` module and ```tmgmt_memory```. 
With this small additions, I managed to speed up my development process, but it also allows others to easily reproduce my development status. 
Note: this addition of a dummy segment is just a temporary setup until ```tmgmt_memory``` becomes fully functional.

My main focus was on connecting the plugin with the backend. For this, I used the setup described above with two simple functions from the API - 
```addSegment``` and ```addSegmentTranslation```, to make sure that there is a translation for the segments in the example translatable node. 
After that, I implemented a route for the lookup of a selected segment in the translation memory. I also provided a 
controller to answer the AJAX call that I do from my JS file. To test my API responses I used [Postman](https://www.getpostman.com). This is an 
app that allows you to send POST/GET/PUT/DELETE etc. requests to see API response. The code can be viewed in [this commit](https://github.com/sasanikolic90/CKEditorSegments/commit/75bc39943132341aab74fc0bb98cfac64774184e).

I had a smaller issue here. My first version of the request was synchronous, which is deprecated because of its detrimental effects to the 
end user's experience. In fact, synchronous requests block the execution of the code and waits until we get a valid response from our server. 
This can create "freezing" on the screen and an unresponsive user experience. I fixed it by following this [JSON Http Request example](http://www.w3schools.com/json/json_http.asp). 
I created a callback function that gets the translation of the segment from the memory and displays it as a suggestion in the area below the editor. 
The user can then click on the button and replace the segment with the suggested translation. With a simple right click, it can be marked as completed (and the counter below is increased).
I implemented the context menu item last week - [see here for more details](http://sasanikolic.com/gsoc-2016/gsoc-ckeditor-plugins-3-week).

As my mentor [miro_dietiker](https://www.drupal.org/u/miro_dietiker) pointed out, "classes are pseudo application states and usually done for 
very simple indication so that you can use CSS for applying styles". In other words, since we are building a complex application with various states, 
we will be using data-attributes to address them. So I dropped the class usage and started working on adding more data attributes to segments.
I started with toggling active status when the segment is clicked and adding the source attribute. We will have to store if the translation was 
done by a user, came from a machine translator or from the memory.

There is still work to be done on the data attributes, as seen below. We need to properly define their naming and replace ```ids``` with ```data-ids```.

<img src="/assets/img/posts/data_attributes.png" alt="Fourth version of the plugin" style="display: block; margin: auto; width: 60%;"/>

## Goals for next week
I plan to continue working on implementing the remaining data attributes. Another issue will be to support multiple translation memory 
matches. We will display N results with N buttons to accept each for every translation suggestion.

My main focus will be on fixing the markup to do real pairing of the editors, because we have to support more than 2 editors on the page - paragraphs
translation for example might result in many editor instances. For now, we have a for loop that goes through all editor instances
on the page and checks for same segments. This adds lags in interaction and could result in a real bottleneck, that's why it's on top of 
our priority list.

We are also in discussions with my mentors about whether or not to use jQuery helpers in the plugin. Our initial plan was to have it purely written 
in Javascript, but I believe jQuery would provide helpers for many DOM lookups and would speed up my development process, other than significantly clean up 
the code.
