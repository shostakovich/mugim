---
layout: post
title: "Command line tappas: Use Pipe Viewer to count new lines per second"
date: 2013-01-23 18:14
comments: true
categories: 
  - command line
  - tappas
  - pipe viewer
---

Often I have to have a look at log files to see how a system behaves. For example recently I wanted to find out, how many points of interes per second were imported.

I used to just count manually. But that didn't really scale well.

So this is what I use now:

    tail -f poi_log | pv -lr >/dev/null

This command uses [PipeViewer][1] to find out how many lines are running through the pipe every second.

What I really like about this is, that you can drill down even further if you use tools like grep. For example its trivial to find out how active bing is on your site.

     tail -f access.log | grep "bingbot" | pv -lr -i 10 >/dev/null
     => [3.97/s ] 

Before you give it a shot you have to install PipeViewer.

Use
    
    apt-get pv

on Ubuntu/Debian or

    brew install pv

on a Mac.

I hopy you like **pv** as much as I do. It's very simple & focused and perfect in everyones toolbox.

[1]: http://www.ivarch.com/programs/quickref/pv.shtml
