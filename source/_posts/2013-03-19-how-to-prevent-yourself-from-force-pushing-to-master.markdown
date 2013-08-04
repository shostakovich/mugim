---
layout: post
title: "How to prevent yourself from force pushing to master"
date: 2013-03-19 20:17
comments: true
categories:
  - git
  - force push
  - dotfiles
  - command line 
---

I am pretty reckless sometimes. I forced pushed to master several days in a row.

What does a real programmer do? - He writes a script that prevents him from doing it ever again, of course..

For the longest time this was a [little complicated][3] - you had to
install a hook onto the remote repro. For Git >1.8.2, there's a pre-push hook that was just perfect for my script.

If the hook returns an error the push is not executed.

## The script

So this is the git hook i came up with. 

{% codeblock .git/hooks/pre-push lang:ruby %}
#!/usr/bin/env ruby
# Pre-push hook that rejects force pushes to master. 
# Requires Git > 1.8.2 

class PushRejecter
  def run
    if pushing_to?(:master) and forced_push?
      reject
    else
      allow
    end
  end
  
  def pushing_to?(branch)
    `git branch | grep "*" | sed "s/* //"`.match(branch.to_s)
  end

  def forced_push?
    `ps -ocommand= -p #{Process.ppid}`.match(/force|pfush/)
  end

  def reject
    puts "Force push to master rejected."
    exit(1)
  end

  def allow
    exit(0)
  end
end

PushRejecter.new.run
{% endcodeblock %}

Ignore the over engineering please..

If the current branch is master and i am force pushing it does return an error and the push is not executed.

## How to install it

All my hooks are stored in a folder called **[~/.git_template][1]**.

It is configured as my template directory for git:

{% codeblock ~/.gitconfig %}
[init]
  templatedir = ~/.git_template
{% endcodeblock %}

In order to activate the hook you have to go into all your existing repros and execute

    git init

This will install the hook for you. As I mentioned you need Git >1.8.2.

Alternatively you could place the hook in the **.git/hooks/** directory manually in each of your repros.

## Conclusion

I am more confident, that I will not cripple a master branch again. A nice feeling. If you like to have a look at the rest of my setup I recommend a look at
[my dotfiles][2].

[1]: https://github.com/shostakovich/dotfiles/tree/master/home/.git_template/hooks
[2]: https://github.com/shostakovich/dotfiles
[3]: http://stackoverflow.com/questions/7419244/elegant-solution-to-prevent-force-push-on-master-only
