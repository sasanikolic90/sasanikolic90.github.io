---
layout: post
title:  "[GSoC 2016] CKEditor plugins - 7. week"
author: sasanikolic
date:   2016-07-12 17:50:00 +0200
category: gsoc-2016
thumbnail: /assets/img/google-summer-of-code.png
keywords: drupalplanet, gsoc, drupal, ckeditor plugins, translation
permalink: /blog/gsoc-week-7/
---
With the time passing by, my final product for the Google Summer of Code 2016 is starting to get the shape. The user's workflow for the translation with [TMGMT](https://www.drupal.org/project/tmgmt) will be easier than ever with 
the CKEditor plugins that I am working on for this summer project. I clearly divided the content into segments and displayed them in the editor before the midterm evaluation period in my first segments plugin. You can read more about that in my 
blog post from week 5 - [here](http://sasanikolic.com/gsoc-2016/gsoc-ckeditor-plugins-5-week/). 

The second plugin called ```tags``` is currently being developed. With it, we want to mask HTML tags inside segments, so that the user can get a better idea of the structure of the content, see which tags are set, opened and check, if they are properly closed.

## Achievements
The seventh week was in a sign of blockers - parts of code that are complex, not yet implemented, not functioning properly or just simply need discussion. I started the week with some code refactoring. 

We have discussed the structure of the masked tags. We do not support masked tag pairs. Instead, every masked tag will consist of two parts - their opening and closing element.
 
* element - the name of the masked HTML tag
* raw - contains the encoded tag, together with attributes  
 
This is is our definition of the masked tag's structure:

```html

<tmgmt-tag element=”b” raw=”&lt;b&gt;”>This is a masked tag<tmgmt-tag element=”b” raw=”&lt;/b&gt;”>
<tmgmt-tag element=”img” raw=”&lt;img src=... alt=... title=...&gt;”>

```

The reasons for this redefinition are many. Firstly, it is really important that the editor displays the masked tags properly with their respective names. This is why we have the ```element``` attribute inside the tag. Secondly, the raw attribute contains the encoded tag. 
In the case when the tag has some attributes like ```src```, ```alt``` and ```title```, we will easily get them from here, decode them, and display them to the user when the tag is clicked. The downside is, this makes these attributes untranslatable and they will be just placed 
back untranslated. When unmasking, the process will simply replace ```<tmgmt-tag>``` with its raw property. Last, but not least, this structure helps us when looking for open and closed tags inside segments.

Also note, the tags above are different. The first one is a tag that requires a closing pair, the second one is a single tag (could also be ```<br />```, ```<hr />``` etc.). If the closing pair of a tag would be missing, we'd color the tag's border in red as a sign of warning.

As mentioned before, I stumbled upon some blockers during this week. The first one was an issue related to closing the tags. Since I created some dummy segments with masked tags inside, the editor's behaviour was expected. The tags didn't have their closing elements (```</tmgmt-tag>```),
so they were added automatically. My mentor pointed me out to check how the ```<img>``` tag is defined in CKEditor. We found out that this is defined the ```CKEDITOR.dtd``` object, which holds the representation of the HTML DTD to be used by the editor in its internal operations.
I need to define the tag in the ```$empty``` object, which contains the list of empty (self-closing) elements.

I also started working on the topic of perfect matching. This consists of getting the translation of a segment from the [translation memory](https://www.drupal.org/sandbox/edurenye/2715815) and checking for a full match. This means, that the text and the HTML structure of a selected 
segment both match with the translation in the memory. Fuzzy matching would be, if only one of that would match. The segments in the memory are unmasked, while we can only get the masked segments from the editor and send them through the HTTP request to the service. 

The workflow of masking the tags would be the following:

1. Mask the tags when the editor loads.
2. Send the masked tag through the HTTP request to the service controller. 
3. Unmask the tag.
4. Query for the translation in the memory.
5. If we have a match, mask it.
6. Return it.

The problem is that the masking and unmasking functions are not supported in tmgmt yet. Remember, I wrote the dummy segments with some masked tags in the ```tmgmt_ckeditor.install``` file solely for the purpose of testing. 
Hopefully, this will be done and committed in the following days, so I can continue my work on that part.

I also did some improvements, like fixing the dependency to the segments plugin, display the tags in the editor pairs properly and  made the tags look nicer by adding some CSS styles.

## Goals for next week
The plan for next week is to fix the blockers described above and to fully restructure my code. I created a prototype object last week ([described here](http://sasanikolic.com/gsoc-2016/gsoc-ckeditor-plugins-6-week/)) containing the relevant information of the editor pair. 
We want to implement all the relevant functions as prototype methods and have them accessible for both plugins, to prevent code duplications and make the code cleaner. 

My GSoC project is available in my [Github repository](https://github.com/sasanikolic90/tmgmt_ckeditor/).
