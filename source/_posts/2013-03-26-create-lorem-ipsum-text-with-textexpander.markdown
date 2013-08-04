---
layout: post
title: "Create lorem ipsum text with TextExpander and lorem"
date: 2013-03-26 18:44
comments: true
categories:
  - tapas
  - command line
  - gem
  - tools
  - mac
  - software
  - productivity
---

Often I need dummy text to fill out a form. 

I use a combination of TextExpander and the Lorem Gem that makes this
super simple.

## Step 1: Install the "lorem" gem

Install the gem **[lorem][1]** using

    sudo gem install lorem

It's simple to use:

    # For 1 paragraph dummy text
    lorem 1 paragraph

    # For 140 chars dummy text
    lorem 140 chars

Lorem is a Unix tool - you can combine it with other commands:

    lorem 140 chars | pbcopy

Copies 140 chars into your clipboard.

## Textexpander + Lorem

I dont want to go into my Terminal, whenever I fill out a
form in Chrome, so I created a [TextExpander][2] snippet, that executes lorem
for me and fills in the dummy text.

{% img /images/uploads/2013-03/lorem_and_textexpander.jpg  "The Lorem gem and TextExpander - a dream team" %}

    #!/bin/bash
    lorem %filltext:name=paragraphs:default=1%

A nice additional feature is that it asks you how many paragraphs of dummy text you want.

## Conclusion

Sometimes simple hacks like this save you a lot of time. I hope you find it as useful as me!

[1]: https://github.com/jnunemaker/lorem
[2]: http://smilesoftware.com/TextExpander/index.html
