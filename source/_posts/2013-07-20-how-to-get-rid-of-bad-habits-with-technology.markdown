---
layout: post
title: "Fighting bad habits with technology"
date: 2013-07-20 19:19
comments: true
categories:
  - productivity
  - command line
  - vim
  - programming
  - twitter
---

I keep a list of stuff to improve. Whenever I catch something stupid it lands in there.

I try to get rid of these bad habits.

Often I find myself unable to solve the problem with willpower alone. Then I search for a solution that assists me. Maybe a little "barrier" or some kind of reminder. Let me show you, what I mean with this..


### Looking at the keys all the time

When I started to program 5 years ago I looked onto my keyboard all the time. Sure - typing faster does not make you a better programmer - but typing with 2 fingers is not ergonomic.

I found a nice solution for my problem.

{% img /images/uploads/2013-07/das-keyboard.jpg %}

I bought a keyboard without letters. I was so bad to hit the proper keys, that nudged me towards learning it right. I have to admit though that I still have problems with some special keys ;)

### Not moving enough during work

Like most people I had the problem that I did not take enough breaks. This was hard to change. 

I started to work in Pomodoros (25 minutes of work and 5 minutes of break). Unfortunately I still did not leave my working space often enough.

So I took another iteration and added added a little AppleScript to my Pomodoro App..

{% img /images/uploads/2013-07/pomodoro.jpg %}

After each Pomodoro my Pomodoro timer executes [a little script that locks my Mac][1] ;)

### Surfing the web all the time

Unfortunately I am addicted to the internet. I am pretty OK with it most of the time — but not, when I want to be productive. 

That's why i wrote a [little script that blocks my internet access][2].

{% codeblock freedom.sh lang:bash %}
#!/bin/bash
echo "Enough of this filthy internet" | cowsay -s
sudo route -n delete default &> /dev/null
{% endcodeblock %}

If I need to be more specific with the site I need to block, I use a program called [SelfControl][3].

This works great! Of course as a computer guy I could circumvent this — but I do not want to.

### Force pushing to master

It happened frequently that I forced pushed to master. This was really annoying for me.

{% img /images/uploads/2013-07/force-push.jpg %}
Image Source:  Flickr / [Pascal][5] ([CC BY 2.0][6])

My colleague [Nikolay][8] proposed to fix it with a script. [That's just what I did][9].

I have not force pushed to master since. This script is now also in use at some of my colleagues.

### Checking working email at home

I checked working email at home. I love my work to much to let my fingers from it - but I realize that I need time of it in order to rest. 

{% img /images/uploads/2013-07/email.jpg %}
Image Source:  Flickr / [Tama Leaver][7] ([CC BY 2.0][6])


There's a simple remedy for not reading work related emails at home: Delete the freaking email account on your computers at home. It's baffling how often you will look into your empty email-client thereafter..

### Checking Twitter really often

{% img /images/uploads/2013-01/tweetbot_filter.jpg %}

There's so much chatter at Twitter. I checked Twitter hourly. Here's what I did:

  * Remove people that write to much
  * Add a URL block-filter
  * Delete Twitter from my iPhone

  This works really well for me. 

### Not using arrow keys in VIM

Another stupid habit of mine was, that I used the arrow keys in order to move in VIM.

This was pretty simple to fix: I disabled them!

{% codeblock ~/.vimrc %}

 not use the arrow keys any more
 nnoremap <up>    <nop>
 nnoremap <down>  <nop>
 nnoremap <left>  <nop>
 nnoremap <right> <nop>
 inoremap <up>    <nop>
 inoremap <down>  <nop>
 inoremap <left>  <nop>
 inoremap <right> <nop>
 {% endcodeblock %}

It took only a week until I was not trying to hit them any more ;)

### Being to digital

Another problem of me is, that I use all my electronic devices to much. Here is what I build to prevent this: __Productivity Protector 2000__.


{% img /images/uploads/2013-07/productivity-protector.jpg %}

It works.

## Conclusion


What is my point to show you this little collection of personal hacks?

Do not be a victim of you're bad habits.

As Nerd we have superpowers: We analyze problems. We solve them. All day long. Start applying these skills to the rest of your life!

  [1]: http://mug.im/blog/2013/01/20/lock-your-mac-from-the-command-line/
  [2]: http://mug.im/blog/2012/09/30/script-to-disable-internet-connectivity-for-mac-os-x/
  [3]: http://www.selfcontrolapp.com/
  [5]: http://www.flickr.com/photos/pasukaru76/
  [6]: http://creativecommons.org/licenses/by/2.0/deed.de
  [7]: http://www.flickr.com/photos/tamaleaver/
  [8]: http://blog.nistu.de/
  [9]: https://mug.im/blog/2013/03/19/how-to-prevent-yourself-from-force-pushing-to-master/
