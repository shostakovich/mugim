---
layout: post
title: "Command Line Tapas: GoAccess - a neat programm to browse through your access log"
date: 2013-01-24 19:14
comments: true
categories: 
  - access log
  - command line
  - tapas
  - goaccess
---

Today I want to tell you about [GoAccess][1], a brilliant log file analyzer:

> [GoAccess][1] is an open source real-time web log analyzer and interactive viewer that runs in a terminal in *nix systems. It provides fast and valuable HTTP statistics for system administrators that require a visual server report on the fly.


You use goaccess like this:

    goaccess -a -f access_log

And end up with a report looking like this:

{% img /images/uploads/2013-01/goaccess.jpg GoAccess main view %}

You see a list of differnet reports and you can drill down further by selecting a specifig report.

{% img /images/uploads/2013-01/goaccess_detail.jpg GoAccess detailed report %}

You can also use goaccess also to use html reports that look like this:

{% img /images/uploads/2013-01/goaccess_html.jpg GoAccess html report %}

I really use this little gem pretty regullary, especially for my blogs.

To give it a shot use the packet manager of your choice or [compile it from source][2].

[1]: http://goaccess.prosoftcorp.com/
[2]: http://goaccess.prosoftcorp.com/download
