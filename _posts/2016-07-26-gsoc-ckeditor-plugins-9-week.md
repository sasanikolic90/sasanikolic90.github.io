---
layout: post
title:  "[GSoC 2016] CKEditor plugins - 9. week"
author: sasanikolic
date:   2016-07-26 18:30:00 +0200
category: gsoc-2016
thumbnail: /assets/img/google-summer-of-code.png
keywords: drupalplanet, gsoc, drupal, ckeditor plugins, translation
permalink: /blog/gsoc-week-9/
---
We are already at the end of July and we just passed the ninth week of coding for the Google Summer of Code 2016 program, which I'm also part of. I am working on extending the Translation Management module with new, very useful functionalities that users will love. Specifically, 
I am extending the CKEditor with new plugins for easier segmentation of the content and masking the HTML tags that are inside them. This will result in a much nicer user experience in translating the content by the local translators.

My current work is based on the masked HTML tags. The masking was properly defined and implemented in the previews weeks. More info about the structure can be found [here](http://sasanikolic.com/gsoc-2016/gsoc-ckeditor-plugins-7-week/). During that implementation I encountered 
a few issues, which I managed to solve successfully. This was thourougly described in my [latest blog post](http://sasanikolic.com/gsoc-2016/gsoc-ckeditor-plugins-8-week/).
Below is a description of the progress that I achieved this week.

## Achievements
As planned, I started this week by implementing a validation of the masked tags. This consists of looping over every segment, displaying the counter of found missing tags in the translation editor based on the source editor, and the array of missing tags. For the purpose of testing, I removed some tags from the dummy
segments, that were stored in the translation memory. Both, the counter and the exact missing tags are displayed in the area below the editor, as soon as a missing tag is detected. 

This means, that we do the validation at many levels:

- when the editors are loaded
- when we add a suggestion from the memory
- when we detect that the content was changed with the ```CKEDITOR.on('change')``` event. 

The benefit of displaying the missing tags is crucial here, so that the translator gets warned about the possibility to have an incorrect DOM structure of the translated text. Later on 
we might add an option for the user to click on a missing tag and _drag&drop_ it into the editor.

My focus this week was also on converting the tags to widgets. This is needed because the DOM structure of the content that the browser renders was completely wrong - the closing tags were automatically added. As [Reinmar](http://stackoverflow.com/users/1464696/reinmar) 
pointed out in [this thread](http://stackoverflow.com/questions/29581028/problems-with-a-custom-self-closing-tag-in-ckeditor):

> You need to implement your <cut> tag as a widget. You already configured your DTD pretty well (you only forgot to set in what elements the <cut> element can exist and whether it is more like a block or inline element), 
> so the parser will accept it and handle as an empty tag. Now you need to wrap it with a widget in order to isolate it so it does not break the editing experience. That should do the trick.

So, firstly, I managed to configure our ```tmgmt-segment``` and ```tmgmt-tag``` elements in the DTD. My idea was to create a block element - the segment, and an inline element - the tag. This worked completely fine. After I converted the tag to an inline widget, I found some issues, which 
I unfortunately still need to solve. The widgets are displayed and are completely draggable, which is great, but they are not in the correct place anymore. They are somehow being placed outside of the segments. I will need to investigate this issue a bit more. One solution 
would be to try and convert also the segment to a non-draggable widget, since I have read somewhere that nested widgets are supported in the latest version of CKEditor.

<figure>
    <img src="/assets/img/posts/widgets.png" alt="Tags as widgets">
    <figcaption>The current issue can be seen in this screenshot.</figcaption>
</figure>

The code of my widget progress can be found in my Github account. I created a new branch called widget, which is accessible [here](https://github.com/sasanikolic90/tmgmt_ckeditor/tree/widgets).

## Goals for next week
I will spend an hour or two on trying to solve the issue described above in the next week. 

My main goal will rather be to create a complete overview of what is currently implemented and displayed in the area below the editor, and think about the things that we can add. 

I will also create a mockup of my ideas, how the final product should look like and discuss about that with my mentors. We discussed about adding some kind of a wizard, which would help the user in the translation process by guiding you through the steps of what still needs work 
or review. I will take that in consideration for the mockups.

Last, but not least, I will do some refactoring of a few pieces of code to get a better performance of my module.
