---
layout: post
title: "Save a file as root in vim"
date: 2013-07-06 19:05
comments: true
categories:
  - vim
  - command line
---

I often edit files without having the right permissions in VIM.

![VIM can not safe a file](/images/uploads/2013-07/sudo-vim.jpg)

I changed the file and when VIM told me that I did not have the proper permissions to save it, I quit VIM and entered ``sudo !!`` to try it again.

This was of course stupid of me. Currently I try to be more mindful about this kind of problems and to fix them.

Here's the lines that I just added to my _.vimrc_

    " Use :w!! to save as root!
    cmap w!! w !sudo tee % >/dev/null

When I enter ``:w!!`` now VIM saves the file as root.

I hope I will not encounter this problem again.
