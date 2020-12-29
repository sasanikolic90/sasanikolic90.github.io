---
layout: post
title:  "[GSoC 2016] Getting started"
author: sasanikolic
date:   2016-05-24 10:55:00 +0200
categories: gsoc-2016
keywords: drupalplanet, gsoc, drupal, ckeditor plugins, translation
usemathjax: true
permalink: /blog/gsoc-getting-started/
---
After the accepted student proposals were announced on the Google Summer of Code 2016 site on the 22nd of April, 
the community bonding period started. The idea of this part is to get to know mentors, read documentation, 
get up to speed to begin working on assigned projects.

Since I had university courses and lots of assignments, I found the time to check [drupal.org](https://www.drupal.org/)
frequently to see the issue progress. I already got to know the community from my internship, hence - the bonding was already
established, I just needed remain in contact with it. We often had discussions with my mentor about tools to use, my project timeline, 
our meetings, things that needed to be done before I start, etc. I also connected with other GSoC students and made some friends 
amongst them. Together with [mbovan](https://www.drupal.org/u/mbovan), we shared many information about GSoC, our projects, timelines and
experiences.

# Tools
We made some conclusions about which tools to use:

* PHPStorm

* Typescript

* Atom

* Mattermost/Skype (for communication and meetings)

* Colloquy (IRC)

* Trello board (for tasks management)

* Google docs for meeting notes

# Meetings
We scheduled our meetings every Tuesday at 15:30 (CEST).

# Details
I was mostly in discussion with [edurenye](https://www.drupal.org/u/edurenye). He made 
[this sandbox](https://www.drupal.org/sandbox/edurenye/2715815), on which my code will be based - this is the backend 
part of the next step we want to do in TMGMT. Like I described in 
[my previous post](http://sasanikolic90.github.io/gsoc/google/project/2016/05/24/gsoc.html), we would 
like to implement an alternative to WYSYWIG editor. With different CKEditor libraries we want to provide some 
special linguistic actions, like defining specific segments and indicate their translation quality, give users alternative
suggestions from translation memory, etc. The segmentation part is done by now, so I can start working on the frontend part - 
building the plugin for the CKEditor.

# This week's goals
For the first week I was planning to start with the first plugin for segments. The plan is to detect a segment without 
changing the source dom, indicate it and detect its context by displaying it in an area below the editor. 
Later on more functionalities will be added.

In the next topic I will describe what I learnt in the first week and the following steps. Stay tuned!
