---
layout: post
title: "Create lorem ipsum text with TextExpander and lorem"
date: 2013-03-26 18:44
comments: true
categories:
  - tappas
  - command line
  - gem
  - tools
  - mac
  - software
  - productivity
---

Sometimes I just need some text to fill out a form. I use a nice way to automate this. In this post I describe how I did it.

## Step 1: Install the "lorem" gem

You can install the gem **[lorem][1]** using

    sudo gem install lorem

It's really simple to use:

    # For 1 paragraph dummy text
    lorem 1 paragraph

    # For 140 chars dummy text
    lorem 140 chars

Lorem is a real Unix tool - you can combine it with other commands:

    lorem 140 chars | pbcopy

Copies 140 chars into your clipboard.

## Textexpander + Lorem

Since I do not want to go into my Terminal if I want to fill out a
form in Chrome I created a [TextExpander][2] snippet, that calls lorem
for me and fills the dummy text in, wherever I am.

{% img /images/uploads/2013-03/lorem_and_textexpander.jpg  "The Lorem gem and TextExpander - a dream team" %}

    #!/bin/bash
    lorem %filltext:name=paragraphs:default=1%

It also asks you how many paragraphs of dummy text you want.

## Conclusion

Sometimes simple hacks like this save you a lot of time. I hope you find it as useful as me!

[1]: https://github.com/jnunemaker/lorem
[2]: http://smilesoftware.com/TextExpander/index.html
