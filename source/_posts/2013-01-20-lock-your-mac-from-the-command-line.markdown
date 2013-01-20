---
layout: post
title: "Lock your mac from the command line"
date: 2013-01-20 13:08
comments: true
categories: 
  - break
  - pomodoro
  - lock screen
  - mac os
  - command line
---

During my daily work I try to take breaks and move frequently. In order for
me to leave the room I have to ensure that my computer is locked.

I wrote a little script which does exactly this.

I only have to enter

    lock

and my Mac starts to lock.

A nice varitation of this is to use this functionallity together with
my Pomodoro app. It locks my computer after 25 minutes of work for me.
Thats a pretty effective reminder to move ;)

Enough said. Here is the code:

{% codeblock lang:bash %}
#!/bin/sh
/System/Library/CoreServices/Menu\
Extras/User.menu/Contents/Resources/CGSession -suspend
{% endcodeblock %}
