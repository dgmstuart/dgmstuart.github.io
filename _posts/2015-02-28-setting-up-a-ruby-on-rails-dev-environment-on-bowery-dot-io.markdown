---
layout: post
title: "Setting up a Ruby dev environment on Bowery.io"
date: 2015-02-28 01:27:44 +0000
comments: true
category: ruby
tags: [ruby, ubuntu]
---
_EDIT: Apparently it's not possible to run docker inside a Bowery instance
because Bowery uses docker and docker has a hard time running inside a docker
container - see [this post on the forum](https://groups.google.com/forum/#!topic/bowery/WsXWzWghwVc)_

[Bowery.io](http://bowery.io/) is a hosted development environment service.
The idea is that you edit your files locally, but run your code in a cloud-hosted
[Docker](https://www.docker.com/) container, based on an image which can be
shared and edited by teams.

It looks like it used to host its own packages and allow environments to be
set up through a gui, but now the approach is to either install everything by
hand or use a dockerfile, which I guess makes sense as docker has become more
and more popular and Bowery's main audience is going to be quite devvy devs
who are comfortable using docker.

I had a go at setting up an environment for ruby development and didn't really
find any documentation so here's what I tried:

Approach: Use a dockerfile
--------------------------
The simplest way to start your Bowery image with ruby is to use a dockerfile
which sets ruby up for you. This can be as simple as creating a file called
`Dockerfile` at the root of your project, consisting of this line:

    from ruby:2.2.0

...then when you select your code directory for the first time through the
Bowery app you can click "Yes" on this prompt to initialize your Bowery image
based on that Dockerfile:

!["A screenshot of the Bowery prompt"](/images/content/bowery_prompt.png)

More details about this approach can be found [on the Bowery blog](http://bowery.io/posts/dockerfile-support/)

Approach: Install ruby directly
-------------------------------
Not having ever used docker in anger, my first attempt involved just
installing ruby manually. The following steps are
basically a textbook set of steps for installing ruby on a new Ubuntu machine
(which is what Bowery instances are based on):

1. Update apt-get:

        apt-get update

2. [Install the dependencies for ruby-build](https://github.com/sstephenson/ruby-build/wiki#suggested-build-environment)
using apt-get

2. [Install rbenv](https://github.com/sstephenson/rbenv)

3. [Install ruby-build](https://github.com/sstephenson/ruby-build)

4. Install ruby with rbenv:

        rbenv install 2.2.0
        rbenv global 2.2.0

5. Install bundler:

        gem install bundler

What about Rails?
-----------------
I _have_ managed to successfully set up a Rails dev environment using Bowery,
but it ended up being pretty fiddly and I'm not sure I have anything coherent
enough to blog about.

I essentially set it up just like any other ubuntu box would be set up (but
I'm still pretty green at server management - hence fiddly).

What I was hoping
to find was a Dockerfile which at least set up Ruby and Postgres, but it seems
like the normal approach is to run separate Docker containers and link them
(as in [this tutorial](http://allenwei.cn/setup-rails-development-environment-with-docker/)).
I don't think this approach applies to Bowery, where a single dockerfile is
used to initialize the image. I suppose I could run docker on Bowery though? (#meta)

In any case, here are the additional components which need to be installed to
run my Rails dev environment:

  * A Javascript runtime (I installed Node.js with apt-get)
  * A database (I installed Postgresql with apt-get but had a pain setting it up)
  * A headless browser for javascript specs (I [built phantom.js from
    scratch](http://phantomjs.org/build.html)
