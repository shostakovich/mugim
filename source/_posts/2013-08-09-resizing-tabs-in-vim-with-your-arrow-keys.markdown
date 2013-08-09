---
layout: post
title: "Resizing tabs in VIM with your arrow-keys"
date: 2013-08-09 19:19
comments: true
categories:
  - vim
  - dotfiles
---

It's worth to write about your learning's. Even if they seem small.

Recently [I turned of my arrow keys in VIM][1] and blogged about it.

After reading my article [Philipp Fehre][2] suggested, that I could take this a step further and use the arrow keys for resizing panes.

So I went ahead and copied the following 5 lines from his [.vimrc][3]:

{% codeblock ~/.vimrc %}
" Make arrowkey do something usefull, resize the viewports accordingly
nnoremap <Left> :vertical resize +5<CR>
nnoremap <Right> :vertical resize -5<CR>
nnoremap <Up> :resize +5<CR>
nnoremap <Down> :resize -5<CR>
{% endcodeblock %}

Phil was right! This is a great way of resizing panes.

[1]: https://mug.im/blog/2013/07/05/how-to-not-use-the-arrow-keys-any-more-in-vim/
[2]: http://sideshowcoder.com/
[3]: https://github.com/sideshowcoder/dotvim/blob/master/home/.vimrc 
