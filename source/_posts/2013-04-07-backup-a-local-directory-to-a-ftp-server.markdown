---
layout: post
title: "Backup a local directory to a FTP server"
date: 2013-04-07 08:28
comments: true
categories:
  - command line
  - tapas
  - ftp
  - tools
  - software
---

I had to backup a local directory to a FTP server.

My first impulse was to use Rsync but I was out of luck. Rsync doesn't work over FTP since it is a [protocol of it's own][1].

That's when I stumbled over [lftp][2]:

> [LFTP][2] is a sophisticated ftp/http client, and a file transfer program supporting a number of network protocols. Like BASH, it has job control and uses the readline library for input. It has bookmarks, a built-in mirror command, and can transfer several files in parallel.

In order to use [lftp][2] you have to install it using the package manager of your choice.

    brew install lftp

Now you create a simple script. We use the mirror command here in order to sync a local directory onto the server.

{% codeblock "A simple lftp backup script" lang:bash %}
SERVER="your.ftp.server"
USERNAME="user"
PASSWORD="pass123"

LOCAL_DIR="/source/dir"
REMOTE_DIR="/target/dir"

lftp -c "set ftp:list-options -a;
open ftp://$USERNAME:$PASSWORD@$SERVER;
lcd $LOCAL_DIR;
cd $REMOTE_DIR;
mirror --reverse"
{% endcodeblock %}

The script uses your credentials to log in to the ftp server. The it starts to mirror your local directory to the directory on the server. The flag  "reverse" is appended to the mirror command because per default lftp mirrors the content from the server into your local directory.

The mysterious line

	lftp -c "set ftp:list-options -a;
	
is necessary if you want to transfer hidden files as well and your FTP server does not list them per default.

## Conclusion

Often FTP backup space is one of the cheapest ways to back up a server, that's when lftp comes really handy. It is way slower then Rsync - but  works fine in most situations.

I am glad I have lftp in my toolchain now.

[1]: http://en.wikipedia.org/wiki/Rsync
[2]: http://lftp.yar.ru/
