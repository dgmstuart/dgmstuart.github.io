---
layout: post
title: "Managing identity in git"
date: 2015-02-04 00:17:21 +0000
comments: true
category: tools
tags: [git]
---

When using git, your commits are labelled with your name and email address.
These are usually based on the `user.name` and `user.email` fields in a global `.gitconfig` file in your home directory,
ensuring that these values are always available.

There might be some circumstances under which these need to vary. For example, on public projects for work I like to use my work email rather than my personal one.
There are a couple of ways to achieve this:

### 1. As an argument to the commit:

    git commit user.email duncan@work.com

This is useful for one-off commits but would be tedious to type every time.

### 2. In a local git config

    git config user.email duncan@work.com

This creates a local `.gitconfig` file. Useful in project directories - since all child directories will use settings in this file,
or will fall back to the global settings if a setting isn't present in this file.

### 3. In an environment variable

    export AUTHOR_EMAIL=duncan@work.com
    export COMMITTER_EMAIL=duncan@work.com

These override everything - any email setting in any `.gitconfig` file.
Useful if like me you like to [manage your dotfiles in git](https://github.com/dgmstuart/dotfiles)
and share them on github in order to easily copy settings between different computers.

These could be set in a local `.profile` file to make every project on that computer use a different email to your main one, while keeping the `.gitconfig` file pristine.
_To understand the difference between Author and Committer see [this StackOverflow post](https://stackoverflow.com/questions/18750808/difference-between-author-and-committer-in-git/18754896#18754896)_

Bonus: What if you don't set anything?
-------------------------------------
It turns out git will try it's best to find a name and email address to associate with your commits even if you don't tell it explicitly what these are.
Here's where it looks, in order, for an email address to use:

1. An `EMAIL` local variable
2. Your system username `@` the value in `/etc/mailname` (which only exists on Debian systems?)
3. Your system username `@` the fully qualified hostname of the computer

For more information check out the [git documentation on the topic](http://git-scm.com/docs/git-commit-tree.html)

_Thanks to [@holizz](https://twitter.com/holizz) for helping me understand all this_
