---
layout: post
title: "Migrating From Octopress 2 to 3"
date: 2016-01-22T00:40:02+00:00
category: octopress
tags: [octopress, migration]
---

I've just migrated this blog from Octopress 2 to 3 and since [there doesn't seem to be a definitive migration guide yet](https://github.com/octopress/octoress/issues/29) (and inspired by a similar post by [@samwize](http://samwize.com/2015/09/30/migrating-octopress-2-to-octopress-3/))
I wanted to share what I did.

This was done with Octopress 3.0.11 and Jekyll 3.0.2. You can see the [actual
commits on github](https://github.com/dgmstuart/blog_source/commits/master).

The approach is to create a new Octopress 3 blog from scratch, import the exising content, and configure a few things to make that existing content work.

## Set up a new blog

First I need to make sure I have the latest version of Octopress available on my machine:

    $ gem update octopress


Now I can make a new blog:

    $ octopress new blog
    New jekyll site installed in /Users/dxwduncan/dev/Personal/blog.
    Added Octopress scaffold:
     + _templates/
     +   draft
     +   page
     +   post

Looking at what that's given us, some of the directories are familar from the `source` directory of Octopress 2 sites (e.g. `_posts`):

    .
    ├── _config.yml
    ├── _includes
    │   ├── footer.html
    │   ├── head.html
    │   ├── header.html
    │   ├── icon-github.html
    │   ├── icon-github.svg
    │   ├── icon-twitter.html
    │   └── icon-twitter.svg
    ├── _layouts
    │   ├── default.html
    │   ├── page.html
    │   └── post.html
    ├── _posts
    │   └── 2016-01-21-welcome-to-jekyll.markdown
    ├── _sass
    │   ├── _base.scss
    │   ├── _layout.scss
    │   └── _syntax-highlighting.scss
    ├── _templates
    │   ├── draft
    │   ├── page
    │   └── post
    ├── about.md
    ├── css
    │   └── main.scss
    ├── feed.xml
    └── index.html

I can now preview the site:

    $ cd blog
    $ jekyll serve

Here's what we get:

!["Jekyll new"](/images/content/jekyll_new.png)


## Configuring gems

Now I want to add a Gemfile (`bundle init`) to manage the versions of Jekyll and Octopress:

{% highlight ruby %}
# Gemfile
source "https://rubygems.org"

gem 'jekyll', '~> 3.0.2'

group :jekyll_plugins do
  gem 'octopress', '~> 3.0.11'
end
{% endhighlight %}

Now I can start copying over my content. I only have posts and a couple of associated images:

    $ cp -r ../blog_old/source/_posts ./_posts
    $ mkdir images
    $ cp -r ../blog_old/source/images/content ./images/content

_N.B. the new blog structure doesn't have an images directory by default_

Now trying to run `jekyll serve` I get an error:

    Liquid Exception: Liquid syntax error: Unknown tag 'codeblock' in /Users/dxwduncan/dev/Personal/blog/_posts/2014-02-07-getting-to-grips-with-postgres.markdown
    jekyll 3.0.2 | Error:  Liquid syntax error: Unknown tag 'codeblock'

This is because I'm using the [codeblock](https://github.com/octopress/codeblock) octopress plugin. There are a couple of different ways of [adding plugins to jekyll](https://jekyllrb.com/docs/plugins/) but the one which makes most sense to me is to put them in the `Gemfile`.

Here's the plugins part of my gemfile after adding the required plugins:

{% highlight ruby %}
# Gemfile

group :jekyll_plugins do
  gem 'octopress', '~> 3.0.11'
  gem 'octopress-codeblock', '~> 1.0'
  gem 'octopress-image-tag', '~> 1.1'
end
{% endhighlight %}

Install them with `bundle install`

Codeblock adds a `.code-highlighter-cache` which needs to be added to the `.gitignore` which Octopress created for me.

## Deployment

Now that I've got a working Octopress blog, I can rely on the [standard
deployment
instructions](https://github.com/octopress/octopress#deploying-your-site).
This is pretty nice now!

## Making it all look nice

Now that all the content is imported it's just a case of editing the `config.yml` file (very minimal compared to the Octopress 2 default) and tweaking the templates.

It doesn't look like there's any obvious way to re-use Octopress 2 themes directly - I suppose it would be possible to copy the styles and templates into the right places, but I fancy a change of look anyway so I'm off to have a look at some Jekyll themes. I hope you've found this useful.
