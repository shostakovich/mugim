---
layout: post
title: "How to deploy Octopress on a Raspberry Pi"
date: 2013-01-20 16:12
comments: true
categories:
  - raspberry pi
  - webserver
  - command line
---

The Raspberry PI is a really inexpensive little computer. It has an ARM
processor, 512 MB of RAM and it runs Linux. I use it to host my website
at home. It's a perfect fit for Octopress.

In this article I explain, how you I set it up. I assume that its pretty
simple for everyone using Octopress of course ;)

## Bootstrapping the Raspberry Pi

First download Raspbian. It's a variant of Debian, that is optimized for
the Raspberry Pi.

    curl http://www.mirrorservice.org/sites/downloads.raspberrypi.org/images/raspbian/2012-12-16-wheezy-raspbian/2012-12-16-wheezy-raspbian.zip

Unzip it:

    unzip 2012-12-16-wheezy-raspbian.zip

And then transfer it onto a SD-Card.

    sudo dd bs=1m if=2012-12-16-wheezy-raspbian.img of=/dev/rdisk2
    
Now put the SD-Card into your Raspberry Pi and boot.

Afterwards you should change some defaults in the configuration tool for
your RaspberryPi.

    sudo raspi-config

{% img /images/uploads/2013-01/pi.jpg %}

* Change the memory split to 16 MB (your server does not need fancy
  graphics)
* Expand the root partition to fill your SD-Card
* Change the password for the pi user

Reboot:

    sudo reboot now

## Time for SSH

Now is the time to use your bigger computer and SSH into your
Raspberry Pi.

{% img /images/uploads/2013-01/ifconfig.jpg %}

Look up the ip for eth0. In my case it's **192.168.2.102**. Another possibility is to use the Web Interface of your router. Usually you should see the IP there as well.

Now log into your pi.

    ssh pi@THEIP
    
## The webserver
  
We need a webserver, so lets install one.

    sudo aptitude install apache2

Check if it works - enter the IP of your Raspberry Pi into your
Webbrowser. You should see **It Works**.

## Rsync

Octopress uses rsync in order to deploy its files. You can install it with:

    sudo aptitude install rsync

## Octopress

Now its time to configure octopress.

    cd octopress
    vim Rakefile

Change the Rsync options to:

    ## -- Rsync Deploy config --
    ssh_user       = "pi@THE_IP_OF_YOUR_RASPBERRY_PI"
    ssh_port       = "22"
    document_root  = "/var/www/"
    rsync_delete   = false
    rsync_args     = ""  # Any extra arguments to pass to rsync
    deploy_default = "rsync"

## Add the authorized key

Copy your public key into your clipboard. If you do not have one yet, [you have to generate a key-pair](https://help.github.com/articles/generating-ssh-keys).

Your key should be stored in ~/.ssh and end with a **.pub**
   
    ls ~/.ssh/

Copy it into your clipboard.

Then add it to your Raspberry Pi

    cd ~
    mkdir .ssh
    nano ~/.ssh/authorized_keys

Paste your key and save. Then adjust the permissions.

    chmod 600 .ssh/authorized_keys

## Deploy

We're nearly done now. The webserver runs. You should be able to login
into you Raspberry Pi without a password and rsync is ready to go. So
lets deploy.

    cd octopress
    bundle exec rake gen_deploy

Now reload the browser again.

{% img /images/uploads/2013-01/raspberry_pi_done.jpg %}

Tada! You're done. You should see your website on the RaspberryPi now.
