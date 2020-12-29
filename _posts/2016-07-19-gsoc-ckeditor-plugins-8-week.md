---
layout: post
title:  "[GSoC 2016] CKEditor plugins - 8. week"
author: sasanikolic
date:   2016-07-19 18:30:00 +0200
category: gsoc-2016
thumbnail: /assets/img/google-summer-of-code.png
keywords: drupalplanet, gsoc, drupal, ckeditor plugins, translation
permalink: /blog/gsoc-week-8/
---
The eight week of my Google Summer of Code 2016 development process is over and the [Translation Management module](https://www.drupal.org/project/tmgmt) for Drupal 8 is closer to becoming a much much better CAT tool than it ever was. My contribution to this project
consists of developing CKEditor plugins for displaying pieces of content - segments, and masked tags inside them. Since we already passed the first midterm evaluation period, we are slowly but steadily moving forward the end goal of having a functional product that 
the open source community can use.

Currently, I am working on the second plugin for masking ```HTML tags``` that are inside segments. The purpose of this plugin is to easily check the structure of the content and be warned of any missing or unclosed tags.

## Achievements
[Last week](http://sasanikolic.com/gsoc-2016/gsoc-ckeditor-plugins-7-week/) I found some important blockers, which prevented me to make significant progress. 

I successfully managed to fix the one regarding the matching of segments containing masked tags. 
The idea behind the translation of segments is to get the active segment, together with the masked ```HTML tags``` inside it and make a ```HTTP request``` to the [translation memory](https://www.drupal.org/sandbox/edurenye/2715815). If one or more *perfect* matches are found, 
we display that as a suggestion and the user can then easily replace the text the chosen one. The main issue I faced here was the fact that the masking and unmasking of the tags was not yet supported by TMGMT. Together with the *MD Systems team* we worked on this part, 
and the final patch for [this issue](https://www.drupal.org/node/2757813) should be committed soon. My mentors, [Miro Dietiker](https://www.drupal.org/u/miro_dietiker) and [Sascha Grossenbacher](https://www.drupal.org/u/berdir) were providing constant support and 
guidelines in finding the optimal solution.

Once that was done, I could work on top of it and define a new workflow with one additional step:

1. Masking the tags with TMGMT.
2. Encode the masked tag before sending it through the HTTP request to the service controller.
3. Unmask the tag.
4. Query the segment with the unmasked tag in the translation memory.
5. If we have a match, mask it and return it.

Because the segments in the memory are not masked, we need to properly invoke the mask and unmask function the service controller. The problem here was that we cannot call alter hooks from my controller. As a quick solution, we provided them as simple external functions, which 
can then be called in the ```hook_tmgmt_data_item_text_output_alter()``` and ```hook_tmgmt_data_item_text_input_alter()``` hooks respectively to mask the tags on load and unmask on save, but also in the controller.

The second issue that I stumbled upon last week was related to the closing tags. Since we defined our custom tag - ```tmgmt-tag```, it needed to be added to the list of empty (self-closing) elements in the ```CKEDITOR.dtd```object. This only solved the issue partially. Yes, 
the source in the editor shows the HTML structure completely right, but the DOM structure that the browser renders is still wrong. I found only [one forum thread](http://stackoverflow.com/questions/29581028/problems-with-a-custom-self-closing-tag-in-ckeditor) on 
*stackoverflow* regarding the same issue. The solution would be to implement our ```tmgmt-tag``` as a widget. Regarding the CKEditor API, to do that, CKEditor 4.3 and above is needed and most importantly, the [Widget plugin](http://ckeditor.com/addon/widget), along with its dependencies.
I tried to implement it, but it seems like that the widget plugin is not part of Drupal 8 yet. 

To solve this, I will follow my two options:

- contact the maintainers of the CKEditor for Drupal to get more info
- include it in my module

Other than fixing last week's issues, I created a new branch in my [Github repository](https://github.com/sasanikolic90/tmgmt_ckeditor/) and started with refactoring. Based on my prototype ```EditorPair```, I changed some functions to prototype methods. The idea here is 
that the functions should not access any data on the page, just do the logic. The prototype and its methods will be moved to a new file, so that the code would be accessible from both modules, ```segments``` and ```tags```. After that, I would use some code review from my mentors here.

## Goals for next week
The main goal for this week is to fix the DOM structure and implement the widget for our custom tag. Once that is done, we can do some nice stuff, like capture clicks and move the tags around and display the data in their ```raw``` attribute. I also plan to do the validation of 
the masked tags. To begin with, a simple counter per segment will be added in both, source and translation editor. Each validation error will be per segment and will be displayed in the area below. I will also display exactly which tags are missing, so that the user gets a 
better idea which tags need to be added. More about it later in the followups.
