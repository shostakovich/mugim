---
layout: post
title: Manage your Mac Apps with homebrew-cask
date: 2013-06-22 11:32
comments: true
categories: 
  - os x
  - alfred
  - mac os x
  - command line
  - devops
---

A while ago I became interested in automating the setup of my Macs. [I started my journey with chef-solo][2], however this wasn't really practical for me, so I [switched to homesick][3] to manage my dotfiles.

Unfortunately that left me without a solution to install my apps automatically.

Not any more. [homebrew-cask][4] to the rescue!

![Take a walk](/images/uploads/2013-06/casks.jpg)

_Image Source: [Flickr/megoizzy][8] (CC BY-SA 2.0)_

## What is homebrew-cask

[homebrew-cask][4] is a [Homebrew][5] extension that allows you to install your apps like this:

    brew cask install evernote
    
## How to install homebrew-cask

If you already use homebrew, installing homebrew-cask is really simple:

    brew tap phinze/homebrew-cask
    brew install brew-cask

If you have [Alfred][6] installed you should also configure it, so that it recognizes apps managed by homebrew-cask.

    brew cask alfred link
    
## Using homebrew-cask

Using homebrew-cask is simple. You install apps using the __brew cask install__ command.  Evernote for example, you would install like this:

    brew cask install evernote

## Create a little script to install your Apps

I store all my apps in a file called __~/.brew__. In order to install the apps I just have to run it.

This is how you create your own:

    touch ~/.brew
    chmod +x ~/.brew
    
Now fill it with the Apps you want to install across all your Mac's. 

For me this looks like:

```
#!/bin/bash
brew cask install google-chrome
…
brew cask install rubymine
brew cask install charles
brew cask install mou
brew cask install bit-torrent-sync
```

I [have this file on GitHub][7] and symlink it with homesick. When I am on a new Mac I just have to run

    ~/.brew
    
That installs all my common apps.
 
## Create own casks

There are over 300 casks available already. You can view a list with

    brew cask search
    
If your favorite app is missing it is really simple to add it.

In the example below I create a Cask for Soulver.

    brew cask create soulver
    
An editor opens with the newly generated cask.

``` 
class Soulver < Cask
   url ''
   homepage ''
   version ''
   sha1 ''
   link ''
end
```

You just have to fill in the download URL of the app, the apps name, the version, the sha1 checksum of the download and it's homepage.

``` 
class Soulver < Cask
  url 'http://www.acqualia.com/files/download.php?product=soulver'
  homepage 'http://www.acqualia.com/soulver/'
  version 'latest'
  no_checksum
  link 'Soulver.app'
end
```
In Soulver's case the download URL does always refer to the latest version. So in this case I used __no_checksum__, so that this Cask will not break in the future.

 Now audit your cask:
 
    $ brew cask audit soulver
    audit for soulver: passed

Now you can install Soulver. And don't forget to open a pull request ;)

Before you do — check the latest [contribution guidelines][1].

## Conclusion

I am really happy that [homebrew-cask][4] exists now. It closed a gap in my configuration management. I can set up a new Mac in no time. I hope you enjoy it as much as I do.

[1]: https://github.com/phinze/homebrew-cask/blob/master/CONTRIBUTING.md
[2]: https://mug.im/blog/2012/10/01/how-to-setup-your-mac-automatically-with-chef/
[3]: https://mug.im/blog/2013/01/22/manage-and-share-your-dotfiles-with-homesick/
[4]: https://github.com/phinze/homebrew-cask
[5]: https://github.com/mxcl/homebrew
[6]: http://www.alfredapp.com/
[7]: https://github.com/shostakovich/dotfiles/blob/master/home/.brew
[8]: http://www.flickr.com/photos/megoizzy/