---
layout: post
title: "PAM and the mysteriously ignored open files limit"
date: 2013-01-26 15:17
comments: true
categories: 
  - pam
  - solr
  - command line
  - chef
---

A few day ago we received a ton of error notifications that looked like
this:

    (RSolr::RequestError) "Solr Response: Lock obtain timed out.

The system ran quite nice for quite a long time - so what was wrong?

It took me quite a while to find the problem. Solr was exceeding the open files limit, so it locked to prevent
problems.

But how the hell did it hit the limit? It's increased on all of our servers. But when I checked it was not applied.

    sudo su USERNAME
    ulimit -a
    ..
    => open files                      (-n) 1024
    ..

The problem was, that I manually restarted Solr a few days earlier, using a rake task. I did not use sudo but su.

If you use **su** the users open file limit will not be applied automatically. Have a look into the PAM configuration for su and guess why..

{% codeblock /etc/pam.d/su %}
#Sets up user limits, please uncomment and read /etc/security/limits.conf
# to enable this functionality.
# (Replaces the use of /etc/limits in old login)
# session    required   pam_limits.so
{% endcodeblock %}

The required line is commented our.

But even more surprising the file limit is activated in most of the other configuration files like **/etc/pam.d/sudo**. So the problem was really me using su to restart Solr.

What I ended up doing was adding this file to chef, with the comment removed. I don't like this kind of inconsistency.
