---
layout: post
title: "How to prevent a cron job from running multiple times"
date: 2013-04-06 17:42
comments: true
categories:
  - command line
  - tappas
---

Sometimes a cron job might run slower, than you anticipated and many instances of it stack up.

You can increase the time interval during starts of the cron job - this can lead to problems in the future however.

Fortunately there's a easy and clean fix.

Just replace your cronjob with this:
    
    /usr/bin/flock -n /var/lock/<foobar> <long-running-task>
    
Whenever your cron job is started a lock file will be created and no other instance can start.

## Conclusion

This a simple remedy for colliding cron job's. I use it frequently.