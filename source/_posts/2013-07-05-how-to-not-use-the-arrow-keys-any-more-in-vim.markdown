---
layout: post
title: How to not use the arrow keys any more in vim
date: 2013-07-05 19:11
comments: true
categories: 
  - vim
  - arrow keys
  - command line
---

I try to wrap my head around the [VIM][1] editor since a few months. Unfortunately I do not have enough discipline to use h-j-k-l as my movement keys. But there is a solution that will teach me..


{% img /images/uploads/2013-07/arrow-keys.jpg %}
Image Source: Flickr / [Colin Harris][3] [(CC BY-NC-ND 2.0)][4]

I found it deep inside the .vimrc of my colleague Fabian.

```
nnoremap <up>    <nop>
nnoremap <down>  <nop>
nnoremap <left>  <nop>
nnoremap <right> <nop>
inoremap <up>    <nop>
inoremap <down>  <nop>
inoremap <left>  <nop>
inoremap <right> <nop>
```

This turns off the arrow keys. A really obvious way of doing it ;)

I will add this to my [.vimrc][2] right away!

Wish me luck!

[1]: http://www.vim.org/
[2]: https://github.com/shostakovich/dotfiles/blob/master/home/.vimrc
[3]: http://www.flickr.com/photos/classblog/
[4]: http://creativecommons.org/licenses/by-nc-nd/2.0/deed.de
