---
layout: post
title: "How to learn VIM"
categories:
  - Workflow
tags:
  - learn
  - vim
---

While [learning Ruby](https://www.thanh.xyz/ruby/how-to-plan-to-learn-ruby-on-rails-in-a-week/), i see some instructor demo writing code using VIM.

Before i know it but just use for some basic editing text in the console, for example on the VPS, there is not IDE/Subline/VSCode.

First time if we look at vim cheatsheet might be a little bit overwhelming at first, so do not try to learn all at once.

My goal is learn first 10 shortcuts key, then apply on a daily basis for work.

### 0. Mode
v: visualize mode
i: insert mode

1. Exit and saving
:w :wq :q! :wq!

### 2. Copy and paste:
v - visualize mode for selecting the text
yy
pp
d

### 3. Moving around
hjkl
:12 go to line 12
0: start of line
G: end of line
gg

### 4. Install plugin:
Edit ~/.vimrc
[https://vimawesome.com/](https://vimawesome.com/)

### 5. Edit
:e
:ls
:sp
:vsp

Benefit of VIM:
- Focus: as you continuous typing, then not switch from windows to windows.
- Super lightweight...

Next, i will try using vim keymap in some modern text editors(vscode, sublime) or RubyMine.
