---
layout: post
title:  "Migrating to Firefox"
date:   2014-07-27 20:52
categories: productivity
---

I've discovered a very powerful firefox add-on,called vimperator.It could 
emulate as a vim editor.It's not only some vim-like hotkeys,but also a 
platform supporting vim-like operations.It has modals(normal mode/command 
mode/insert mode),and it could follow links conveniently.You type f in a
web page,and it show a character on every link,you type the corresponding
key then it acts like you clicked the link with mouse.It's very convenient.

Today,I have migrated to vimperator's fork called pentadactyl.It's more 
powerful than vimperator. One of the most flaws of vimperator is that it
has no convenint way to escape from insert mode.It could escape from insert
mode with esc key,but I'm used to use Ctrl-[ key as esc key,because I use a
MacBook air,the esc key is too small to press.With pentadactyl,it's very
conventional with my previous vim experience.

But,pentadactyl has a flaw too.In its recent commit,it has a feature which
could reduce the height of Firefox's tab bar.But,this feature is imperfect.
It leave a tiny line in the top of the browser window with every website.
I have to search the web,and found a discussion on google group.Folks has
developed a patch to fix this(just discard that commit).I applied that patch
in my local source,and install the local version of pentadactyl,and the
problem fixed,It now becomes a perfect browser for me.And I will discover
more powerful add-ons with Firefox to work more productively.

