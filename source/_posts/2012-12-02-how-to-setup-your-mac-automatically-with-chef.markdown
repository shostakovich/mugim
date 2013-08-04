---
layout: post
title: "How to setup your Mac automatically with chef"
date: 2012-10-01 19:29
comments: true
categories: 
  - chef
  - mac
  - devops
  - command line
  - dotfiles
  - mac os
  - mac
---

Recently I joined a team that practices DevOps. That's why I had to learn Chef:

> With Chef, you write abstract definitions as source code to describe how you want each part of your infrastructure to be built, and then apply those descriptions to individual servers.

Since I regard my iMac as part of my infrastructure - I decided to give Chef a shot. In this post I explain how I did it.

## Bootstrapping chef

Before the first Chef run, you have to prepare youre machine. 

I store my cookbooks on Dropbox. So after installing the OS (manually), I download and install Dropbox.

After Dropbox finished syncing I run bootstrap.sh :

{% codeblock bootstrap.sh %}
#!/bin/sh
USERNAME=shostakovich

mkdir ~/tmp
cd ~/tmp

# Install GCC + Git
curl https://github.com/downloads/kennethreitz/osx-gcc-installer/GCC-10.7-v2.pkg > GCC-10.7-v2.pkg
sudo installer -pkg GCC-10.7-v2.pkg -target /

# Install chef
sudo gem install chef

# Prepare Directory for Homebrew
sudo mkdir /usr/local
sudo chown -R $USERNAME /usr/local
{% endcodeblock %}

This installs a compiler, Homebrew and some dev-tools like GIT on the machine.

Last but not least bootstrap.sh installs Chef itself.

## Installing apps & packages

I have my standard set of tools. Instead of installing them manually I wanted to use Chef.

Luckily there are already cookbooks that can install [dmg's](https://github.com/opscode-cookbooks/dmg) and apps within [zip files](https://github.com/fnichol/chef-zip_app).

You just have to download them and then you can specify which apps to
install:

{% codeblock package_examples.rb %}
dmg_package "Google Chrome" do
  dmg_name "googlechrome"
  source "https://dl-ssl.google.com/chrome/mac/stable/GGRM/googlechrome.dmg"
  action :install
end

zip_app_package "Mou" do
  source "http://mouapp.com/download/Mou.zip"
end

dmg_package "Virtualbox" do
  source "http://dlc.sun.com.edgesuite.net/virtualbox/4.1.18/VirtualBox-4.1.18-78361-OSX.dmg"
  type "mpkg"
end
{% endcodeblock %}

For all my Unix tools I use the [Homebrew cookbook](https://github.com/mathie/chef-homebrew).

This way I can specify all the packages I need in my node.json - the configuration file I use to run chef-solo.

{% codeblock node.json %}
{
  "run_list": [
  	"recipe[main]", 
  	"recipe[git]", 
  	"recipe[rvm]",
  	"recipe[apps]"
  ],
  
  "packages" : [
 	"wget", 
 	"tmux", 
 	"watch", 
 	"mobile-shell", 
 	"imagemagick", 
 	"solr", 
 	"mysql", 
 	"aspell",
 	"htmldoc",
 	"ghostscript",
 	"redis",
 	"pdftohtml"
  ],

  "homedir" : "/Users/shostakovich",
  "user" : "shostakovich"
}
{% endcodeblock %}

## Configuration

I store most of my dotfiles on my Dropbox.

When i set up a new machine i symlink them into my home directory. Last but not least, i change some defaults - for example to move the Dock out of the way..

{% codeblock config_examples.rb %}
template "#{node['homedir']}/.vimrc" do
  source "vimrc.erb"
  owner node['user']
end

execute "set dock to be on left" do
  command "defaults write com.apple.dock orientation -string left"
  user node['user']
end

execute "relaunch dock" do
  command "killall Dock"
end
{% endcodeblock %}

## How can you get started?

I highly recommend giving it a shot! It's a good way to learn chef. 

For a kick start with chef-solo, watch [RailsCast 339][1].

If you have problems setting up chef-solo on a Mac, have a look at [how to fix solo.rb][2].

If you need further ideas, have a look at the  [workstation setup of pivotal][3].

## The end

I used this a couple of times already. Its kind of boring, to watch chef setting up your dev-machine. But hey - you can drink coffee in the meantime.

[1]: http://railscasts.com/episodes/339-chef-solo-basics
[2]: http://woss.name/2011/01/23/converging-your-home-directory-with-chef/
[3]: https://github.com/pivotal/pivotal_workstation
