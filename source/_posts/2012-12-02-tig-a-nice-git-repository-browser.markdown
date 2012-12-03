---
layout: post
title: "tig - a nice git repository browser"
date: 2012-09-30 12:28
comments: true
categories: git
---
A colleague just recommended [tig][1] to me.

{% img /images/uploads/tig.jpg %}

You can install tig with the packetmanager of your choice.

After you  start tig in your project, you see a list of the last commits and can browse through them using your arrow key or the good old vim style (with j, k).

If you hit enter you can inspect the commit in question.

The other nice feature for me is: if you hit "t" you can browse through the tree of files in the project. With "B" you can see do a git blame on for a specific file.

{% img /images/uploads/tig_blame.jpg %}

tig has many more features - just press "h" to get a list of all of them, but for me these are the two killer features. 

tig works better then Tower, Gitbox, or GitK, because I do not need to leave the command line.

If you are interested, visit the [tig website][1] and give it a try.

[1]: http://jonas.nitro.dk/tig/