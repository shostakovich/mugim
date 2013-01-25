---
layout: post
title: "Command line tappas: Feel like a hacker with cluster ssh"
date: 2013-01-25 18:37
comments: true
categories:
  - pair programming
  - tappas
  - command line
  - cluster ssh
  - ssh
  - devops 
---

A few months ago I paired on a chef recipe with a colleague. After we
uploaded the cookbook we wanted to try this stuff out on a few nodes.

Until then I would have kind of done it completely manually. But he
entered a command and the following happend:

{% img /images/uploads/2013-01/csshx.jpg ClusterSSH in action %}

Wow. He just ssh'ed into all of these servers. The next part looked even
cooler. He started typing and magically the text appeared in all the
terminals. He pressed enter and the chef run started. It looked awesome. Chef runs produce quite a lot of text ;)

I was hooked and asked him what he just did. "That's just ClusterSSH", he
replied, "I use it all the time".

> [ClusterSSH][1] is a tool for making the same change on multiple servers at
> the same time. The 'cssh' command opens an administration console and
> an xterm to all specified hosts. Any text typed into the
> administration console is replicated to all windows. All windows may
> also be typed into directly.

For the Mac there is a similar tool called [csshx][2] thats what I ended
up using.

It works like a charm. You just specify your clusters in a clusterfile.

    cluster1 host1 host2
    cluster2 host3 host4

Now you just enter:

    csshx cluster1

That's it.

I use [csshx][2] during every maintainance window. I love
pair programming. Makes you realize that things that are obvious to
yourself are not obvious to your colleagues.

[1]: http://clusterssh.sourceforge.net/
[2]: http://code.google.com/p/csshx/
