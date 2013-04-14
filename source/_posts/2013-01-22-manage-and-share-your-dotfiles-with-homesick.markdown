---
layout: post
title: "My home is my castle - How to manage and share your dotfiles using homesick"
date: 2013-01-22 17:50
comments: true
categories: 
  - command line
  - dotfiles
  - homesick
  - gem
  - tappas
---

{% img /images/uploads/2013-01/castle.jpg 890 668 %}
*Castle 11 from [Bill Ward's Brickpile][1]. (CC BY 2.0)*

Since quite a while I try to find a good solution for sharing my dotfiles between my different computers. 

[I started with chef][2]. - Unfortunately this did not work so well. Especially if you also want to use your dotfiles on a server. Also it feelt a little complicated. 

The next approach I took was putting my dotfiles into my Dropbox and symlink them. This worked ok. But Dropbox on a server? No way. 

So I keept looking. A few days ago I stumbled over
[homesick][3] - an amazing rubygem that manages your dotfiles.

## How to set it up

First install homesick:

    gem install homesick

Now create a "castle". A castle is a collection of dotfiles.

    homesick generate ~/dotfiles

This creates a git repro with a /home folder inside. Just put your dotfiles / dotfolders in there. Then push the stuff to Github.

Now install the castle with homesick.

    homesick clone https://github.com/foo/dotfiles.git

Now let homesick do its magic.

    homesick symlink dotfiles

All the dotfiles from within this castle will be symlinked.

## This is awesome!

* You feel at home at each of your servers in no time
* You share your dotfiles with the world
* [You can learn from others and refine your files][4]
* You can put essential little scripts into a .bin directory.
* The castle concept allows you to try someone elses dotfiles

## A word of caution

Please do not put sensitive informations into you dotfiles.
Especially ssh keys, bash histories and passwords in configuration
files do not belong on Github.

## Try it!

Seriousely. It's magical. This is one of those things that you try out and you ask yourself how you could possibly live without it. 

Interested? Have a look [at my dotfiles][5].

[1]: http://www.flickr.com/photos/billward/3393266991/
[2]: http://www.mug.im/blog/2012/10/01/how-to-setup-your-mac-automatically-with-chef/
[3]: https://github.com/technicalpickles/homesick
[4]: http://dotfiles.github.io/
[5]: https://github.com/shostakovich/dotfiles
