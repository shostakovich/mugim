---
layout: post
title: "How to handle errors like a professional"
date: 2013-06-06 19:44
comments: true
categories:
  - error
  - failure
  - bug
  - programming
  - code
  - software
---

As a good programmer you try to get as much feedback as possible. Be it automated unit tests, code reviews or metrics. For me bugs and failures fall under the same category, if they do not repeat over and over again.

{% img  /images/uploads/2013-06/failure.jpg %}
"Olivier Le Moal/Shutterstock.com"

In an agile environment you will have a lot of opportunities to make errors, and the impact of these errors is relatively small.

By embracing errors and working on avoiding them in the future, you can create a very resilient environment.

In this article I want to explain how you can get the most out of your errors.

## Properties of a great error

Let's start with some properties, of a great error.

* You notice it as soon as possible
* You minimize the impact
* You find out what exactly is wrong
* You learn from it and find a way to avoid it in the future
* You incorporate your learning into your routine

## Noticing problems as soons as possible

Sometimes errors are really obvious. The website doesn't work at all. There is a typo in the imprint. You force pushed to master or you deleted a table in the database.

But often errors are much more subtle. It's worth to consciously look out for problems. The more feedback from your application you can get the better.

That's one reason, why you want good metrics of your applications. You could also monitor your logs and notify the team if something unusual is in there. Or you have a staging environment or/and a canary server. These are all ways to discover problems really fast.

Whenever you notice that a problem went unnoticed for a really long time, this can indicate, that you could improve on that front.

## Keeping the impact of errors minimal

If you discover a problem - don't panic. Take a few deep breaths. Maybe even leave your desk for a minute. Get calm. Grab a coffee.

Then get help. Tell your colleagues about the problem. Either one of them  already exactly knows what is happening or they are a great help at finding the problem.
 
 Even more important: Do not waste your energy to find out who has caused the bug. The person probably already knows ;)
 
 And hey: This is an opportunity to learn what happend and to fix the problem together.
 
In case it's your own error admit it. Admiting errors is not only very professional, it also generates trust and reduces damage. It's also very likelly that you are the one person, who knows exactly where to look. So speak up!

I trust colleagues, who admit errors and accept help - and so should you. They care and take responsibility. They know when to ask for help. No need for blaming.

If you have the possibility to buy time - like rolling back to the last release or toggle a feature switch, you should do this first. The less stress you feel the faster you usually find the problem and the better is the quality of your fix.

## You can find out why the error happened

If the cause isn't obvious you have to look for it. There are tons of ressources about troubleshooting out there.

Some good ones include:

* [Debug it! from PragProg][1]
* [Troubleshooting, The Developer's #1 Skill][2]

Do not stop to early. Really try to find what happens. Even if the error is not that important it is worth to understand what happens.

For example once we noticed unusally high IO after a Rails update. It was not obvious that this was an error. We looked into the issue and found that Rack was caching all our pages on disk. This could have brought down our whole site during a traffic spike. This is also a nice example for a bug that is really hard to catch in advance.

## Not every problem is worth fixing

Not every problem is even worth fixing. But in order to do this assessment ,you need to understand what is going on.

Sometimes a seemingly simple errors, upon further investigation, turns out to be much bigger than anticipated.

## There is something to learn and you learn it

Think about what you can learn from the error. This might be one thing for some problems, but more often you can learn multiple things from one error.

For example take a syntax error that slipped onto production. The lessons could be:

* Why the fuck did not test catch it? We should write more tests..
* Why the hell did no one see this in the code review? Do we need to be more thourough? Or should we automatically run syntax checks?
* Why the hell did I check this in at Sunday at 03:00 am - maybe I should sleep more

Try to come up with many reasons. No single one of them might be

## Avoid it in the future

Now that you have identified, what you could improve - think about if you want to improve it. Try to act on it and avoid the same kind of problems in the future. Also share your insights and a nice description of the problem with other programmers online - or even better keep a little engineers diary and write this stuff down for yourself.

If you use something like a team Wiki and you think you discovered a lesson - put it in there. The next colleague having a similar problem might thank you.

A recent example for such a measure in our team is  the following:
I accidently force pushed to master. First I tried to stop myself soon enough but after being on a cruise I did it again. So I wrote a little git hook that prevents me from doing it. In the future. No force pushing to master from me since.

Some of my colleagues also have installed this hook as a safe guard now.

## Conclusion

An error is no reason for blame - but an opportunity for improvment. Treat it as whar it is - just another kind of feedback that something in your process is not perfect yet.

Incorporate this feedback wisely to avoid the same problems from occuring again. Share your lessons.

How do you handle problems in your environment?

[1]: http://pragprog.com/book/pbdp/debug-it
[2]: https://peepcode.com/products/troubleshooting