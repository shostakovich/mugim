---
layout: post
title: "Productivity: Mute tweets without urls in Tweetbot"
date: 2013-01-27 11:32
comments: true
categories:
  - mute filter
  - twitter
  - regex 
---
{% img left /images/uploads/2013-01/tweetbot_filter.jpg "TweetBot mute filter" %}

I spent far to much time on Twitter lately. Time wasted that should be
used for building stuff. On the other hand I do not
want to complettely abandon Twitter, because it's a great source of
new links for me.

So I decided to filter out all Tweets without URL's.

The regex required came from [stackoverflow][1]
    
    ^((?!http).)*$

Feels much better - and I can follow more great people ;)

[1]: http://stackoverflow.com/questions/406230/regular-expression-to-match-string-not-containing-a-word
