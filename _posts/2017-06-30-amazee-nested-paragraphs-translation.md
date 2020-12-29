---
layout: post
title:  "[Paragraphs] Nested Paragraphs translation"
author: sasanikolic
date:   2017-06-30 12:00:00 +0200
thumbnail: /assets/img/posts/paragraphs-translation-1.png
categories: drupal
keywords: drupalplanet, gsoc, drupal, ckeditor plugins, translation
permalink: /blog/nested-paragraphs-translation/
---
Looking back, translating content in Drupal 7 wasn't a straightforward task. It involved enabling a number of modules to do all the translations properly. In Drupal 8 however, the Multilingual Initiative took big steps forward to enhance multilingual support for users.

Since its introduction in May, 2011, huge efforts by everyone involved resulted in hundreds of issues resolved and many great improvements have since been made that now drastically simplifies the site building process.

Nevertheless, basic Drupal content management can be extended with many modules. One of the most popular is Paragraphs. Instead of putting the content in one WYSIWYG body field, it lets users choose between predefined Paragraph Types. These can be anything from a simple text field to a combination of images, text, videos and slideshows.

We are extensively using Paragraphs at Amazee Labs for our content-rich websites and opted for its use for our first fully-decoupled Drupal project as well.

Paragraphs in Drupal 7 currently lacks support for Entity Translation. However, this has been successfully implemented in its new iteration and I personally contributed in this process. The translation process is not as straightforward as translating basic nodes or other entities and users are still facing many issues. For a better understanding of the topic, a community documentation page for [Paragraphs Multilanguage constraints](https://www.drupal.org/node/2735121) was created on [Drupal.org](https://www.drupal.org).

This page covers only the basic setup and some general constraints. But how do we set it up for complex projects, with many levels of nested paragraphs? Let’s recap the basics first and dig deeper in nested paragraph translation a bit later.

## Basic rules
The Paragraphs module works within a multi-language setup but there are crucial steps in order to achieve the correct functionality. We can split the set up into two parts:

- **Site building:**
  - Translatable paragraph fields on the parent entity are not supported (e.g. Node, Taxonomy term, etc.)
  - Disable translation on an Entity Reference revision field either inside a Content type or Paragraph (untick Users may translate this field)
- **Content language and translation UI:**
  - Disable translation on an Entity Reference revision field (where it says (* unsupported))
  - Enable the translation on every Paragraph

## Example
That being said, let’s look at an example of the correct and incorrect way to enable translatable paragraphs.

The first point says that whenever we create a field that holds paragraph elements on the first level - on a content type, for example, should not be translatable. The example node, in this case, contains a Content field (field_landing_page_content) which is a reference to many other Paragraphs, like Gallery and Accordion. This field must **NOT** be set to translatable - “Users may translate this field” should be **unchecked**!

<figure>
    <img src="/assets/img/posts/paragraphs-translation-1.png" alt="Uncheck users may translate this field">
    <figcaption>"Users may translate this field” should be unchecked!</figcaption>
</figure>

Instead, only Paragraph types that contain content fields, like links, texts or images need to be set to translatable, because this fields itself need to be translated.

To activate the multi-language functionality you need to go to: ```admin/config/regional/content-language```. This step is crucial when dealing with nested Paragraphs. In our case we use a structure with 3 levels of paragraphs that looks like this:

~ **1st Level Page** (Node)

~ ~ **Content** (Entity reference revisions field, not translatable)

~ ~ ~ **Section** (Paragraph, has Entity reference revisions field Content with Accordion Paragraph and others)

~ ~ ~ ~ **Accordion** (Paragraph, has Entity reference revisions field Accordion container with different accordion paragraphs)

~ ~ ~ ~ ~ **Simple Accordion Item** (Paragraph, has Entity reference revisions field Content with different content paragraphs)

~ ~ ~ ~ ~ ~ **Title, Text, Video, Gallery etc.** (Paragraph, text, link and Entity reference (media) fields)

As a first step here, make sure Paragraph is checked.

<figure>
    <img src="/assets/img/posts/paragraphs-translation-2.png" alt="Paragraph should be checked">
    <figcaption>Make sure "Paragraph" is checked.</figcaption>
</figure>

Under the 1st level page, the content should **NOT** be set to translatable. The Paragraphs module is smart enough to warn us about the Entity reference revisions fields being currently unsupported (there is a [big meta ticket](https://www.drupal.org/node/2461695) about this topic on d.o.).

<figure>
    <img src="/assets/img/posts/paragraphs-translation-3.png" alt="Content untranslatable">
    <figcaption>Content should be set to untranslatable.</figcaption>
</figure>

Next, we need to dig into the fun part - the Paragraph section. Let’s look at the example mentioned above. Every Paragraph should be set translatable.

<figure>
    <img src="/assets/img/posts/paragraphs-translation-4.png" alt="Translatable paragraphs">
    <figcaption>Paragraphs should be translated.</figcaption>
</figure>

At least one field should be selected in order for a Paragraph to be translatable. If the Paragraph has only one field, which is of type Entity reference revisions (a linking Paragraph), we need to check some of the default text fields.

<figure>
    <img src="/assets/img/posts/paragraphs-translation-5.png" alt="Select at least one field">
    <figcaption>Select at least one field per Paragraph.</figcaption>
</figure>

Here we want to translate specific text fields as usual.

<figure>
    <img src="/assets/img/posts/paragraphs-translation-6.png" alt="Translating text">
    <figcaption>Text translation in a Paragraph should be always enabled.</figcaption>
</figure>

Saving this configuration should get you up and running and ready to go. Happy Paragraph translating!
