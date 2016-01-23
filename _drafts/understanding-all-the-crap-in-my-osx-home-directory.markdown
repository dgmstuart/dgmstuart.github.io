---
layout: post
title: "Understanding all the crap in my OSX home directory"
date: 2015-02-08 22:11:13 +0100
comments: true
category: git
tags: [git]
---

This is a long overdue post about how I cleaned up my home directory and learned a lot in the process. The story starts with dotfiles.

Dotfiles, or "Why my entire home directory is a git repo"
----------------------------------------------------------
Dotfiles are those files like `.bashrc` with filenames starting with a dot. They mostly live in your home directory (on Unix and OSX) and hold the configuration settings for your command line tools.
[My dotfiles are all on github](https://github.com/dgmstuart/dotfiles) which means I've got a backup, and I easily can share settings between computers and with my team.

There's a [Github page which goes into more detail on why and how to do this](https://dotfiles.github.io/), but suffice it to say that it involves making your entire home directory into a github repo and committing the files you want to save.

At this point you have a decision to make:

1. You can just leave the remaining files uncommitted and decide to not care that you now have a dirty repo on your machine
2. You can ignore all the remaining files in some combination of `.gitignore`, `.git/info/exclude` and potentially even an `excludesfile` (though you're likely to want to be check this in). _See the [git docs](http://git-scm.com/docs/gitignore) for how each of these should be used._
3. As per 2 but carefully considering where each file should be ignored, or if it should just be deleted

Of course I chose 3.

Much googling later, here's what I came up with:

Deleted outright
----------------

#### `.rdebug_hist`

  * History file for a ruby debugger I must have used in the past


####.recently-used.xbel

* Sounds like this might be related to using
[Pidgin](https://pidgin.im/) back in the day. Not sure exactly what it does but I found [a thread about it](http://www.howtogeek.com/howto/16230/what-is-.recently-used.xbel-and-how-do-i-delete-it-for-good/)


#### .gimp-2.4/

* Seems to be all the fonts and brushes etc. used by [GIMP](http://www.gimp.org/).
* Newer versions of Gimp run natively in OSX rather than in X11 - hopefully all this won't be needed.

#### `.thumbnails`

* GIMP's cache of thumbnail images

#### `.gtk-bookmarks`

* Contained a couple of file paths to old website projects.
* Maybe something else related to GIMP

#### `CTX.DAT`

* [Something to do with Citrix sessions](https://discussions.apple.com/thread/1713980?start=0&tstart=0).
* I had to use Citrix in a previous job.

#### `.Xauthority`

* Something to do with ssh-ing onto a remote X11 machine. I don't do that sort of thing these days.




### Recurring Files which I stopped getting generated
I wanted to get rid of the following files, but they kept getting regenerated. I was able to work out how to stop these getting regenerated.

#### `.lesshist`

* Apparently whenever I open something up using less on the command line, and then search within that file, it saves those searches to a file.
* This can be disabled by setting an environment variable: `export LESSHISTFILE="-"`
* [Source1](http://list.freebsd.questions.narkive.com/bqLCfqNE/lesshst), [Source2](http://mail-index.netbsd.org/tech-security/2010/02/15/msg000282.html)


#### `.serverauth.xxxx`

* Seems to be something to do with ssh-ing onto a remote X11 machine ([Source](http://taosecurity.blogspot.co.uk/2006/09/eliminating-serverauth-files.html)).
* Probably related to me fiddling with web hosting in the past.
* To prevent these getting generated, edit your `user/X11/bin/startx` file: change `xserverauthfile=$HOME/.serverauth.$$` to `xserverauthfile=$XAUTHORITY`


Ignored in `.git/info/exclude`
-------------------------------

#### `.zsh-update`

* This is used by .oh-my-zsh to work out whether it should ask you to check for updates: it will ask if the last update was more than 6 days ago. ([Source](https://bitbucket.org/Josh/oh-my-zsh/src/tip/tools/check_for_upgrade.sh))
* It contains a single line, e.g. `LAST_EPOCH=15952`

#### `.heroku/`

* I had assumed this was to do with the [Heroku](https://www.heroku.com) gem, (which has now been replaced by a [standalone cli application](https://github.com/heroku/heroku)), since there seemed to be a lot of redundant stuff in there from back when heroku was a gem - e.g. console history which isn't used by the new heroku client
* When I deleted this it got re-created, so presumably it's still used by the heroku application.


#### `.rnd`

* Seems to be the seed file used by [openssl](https://www.openssl.org/).
* Contains a long string of random characters.

#### `.cups/lpoptions`
* Contains printer settings.
* For me this file has a single line with "Default" followed by the name of a printer.
* [CUPS](https://en.wikipedia.org/wiki/CUPS) is a Unix printing system
* [`lp`](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/lp.1.html#//apple_ref/doc/man/1/lp)
stands for
[Line Printer](https://en.wikipedia.org/wiki/Line_printer) and is the Unix command for submitting files to a printer

#### `.fontconfig/`

* [Fontconfig](http://en.wikipedia.org/wiki/Fontconfig) is a library for providing fonts to other programs.
* Not really sure what this directory does, but it contains a lot of cache files, so I left it alone.

#### `.gem/`

* contains a `credentials` file with my rubygems API key
* Also contains a `/specs` directory. As far as I can understand, when installing or updating system level gems (e.g. gem install x) the gemspecs which rubygems finds are cached here, presumably to make future installs and updates faster


### Files which get regenerated regularly:
The following files really shouldn't get generated as far as I can tell, but they're in the exclude file because no matter what I do, they keep coming back:

#### `.hgignore_global`

This is the [Mercurial](http://mercurial.selenic.com/) equivalent of a global `.gitignore`. I've never used Mercurial, but this file gets regenerated on a regular basis, so there must be some application on my machine which contains a Mercurial repo under the hood and generates this file when updates happen.

#### `.vim/.netrwhist`

* History file for `netrw`
* `netrw` is a vim plugin allowing you to use vim to operate on directories (as well as files).
* Mine tends to contain a couple of config lines, plus one history line
* I haven't consciously used vim to navigate directories so this is - probably a result of some mistake I've made fiddling around with vim foo I didn't understand
* Details on [vim.org](http://www.vim.org/scripts/script.php?script_id=1075) and [Stack Overflow](http://stackoverflow.com/questions/9850360/what-is-netrwhist)


- - - - - - - - - - - -

TO LOOKUP: .dropbox-master/ - seemed to just contain some binary keys.

.netrc

.CFUserTextEncoding

Generated by applications: deleted
----------------------------------

'./config'
  Last.fm
  gtk-2.0 The Gimp ToolKit http://www.gtk.org/



-------

Legit stuff:
.dropbox/
.rvm/
  I guess this needs to live in my home directory because it's specific to me as a user? https://rvm.io/rvm/install

.viminfo seems to contain some interesting historical information

Write something about .zcompdump*
