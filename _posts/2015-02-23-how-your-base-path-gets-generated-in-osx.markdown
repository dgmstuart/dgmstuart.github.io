---
layout: post
title: "How your base PATH gets generated in OSX"
date: 2015-02-23 23:03:33 +0000
comments: true
category: osx
tags: [osx]
---
On OSX and linux it seems to be fairly common practice to totally define the `PATH` in a
`.profile` or `.bashrc` file. This gives total control over the order in which
various locations are searched through. Here's the line from [my .profile](https://github.com/dgmstuart/dotfiles/blob/master/.profile):

    export PATH=~/bin:/usr/local/bin:/usr/bin:/usr/sbin:/bin:/sbin

However, your `PATH` isn't just set through `profile` and `rc` files: if you
don't build your path from scratch and you only ever append or prepend to it
with statements like `export PATH=$PATH:some/other/stuff` then the OS helpfully
sets your base `PATH` to something sensible.

It does this based on various files in `/etc` (at least on OSX):

## `/etc/paths`

This contains the list of the various binary directories which make up the core of your path:

    /usr/local/bin
    /usr/bin
    /bin
    /usr/sbin
    /sbin

## `/etc/paths.d`

This contains a number of files which contain paths for various applications,
again with one path element per line. I believe these are added to the _end_ of
the `PATH`. Mine includes a `git` file:

    /usr/local/git/bin
