---
layout: post
title: "Octopress Is Dead! Long Live Octopress!"
date: 2016-02-18T22:22:54+00:00
---

[Octopress 3](https://github.com/octopress/octopress) exists. Well some of it
does. It's complicated. And it's a totally, fundamentally different kind of
thing to the Octopress you know and love, but everything is going to be OK:
trust me.


## So you think you're an Octopress blogger?

Maybe you're like me: you're a programmer, you heard you should be blogging
and you felt like you needed something more edgy than a
[WordPress.com](https://wordpress.com/) account,
but couldn't be doing with the faff of totally rolling your own.

Or maybe you just wanted to be able to use your
[favourite syntax highlighting theme](http://ethanschoonover.com/solarized)
without jumping through a bunch of hoops.

[Octopress 2](https://github.com/imathis/octopress)
seemed to tick a bunch of boxes:
[lots](http://pragdave.me/)
[of](http://blog.davidchelimsky.net/)
[tech](http://thepugautomatic.com/)
[blogs](http://www.breck-mckye.com/)
seemed to use it (often with the default theme) and despite feeling suitably
hacker-y it had familiar features like plugins and themes.

It also ran on Ruby. I like Ruby.

## The problems

[Brandon Mathis](https://twitter.com/imathis)
the author of Octopress is the first to own up to the issues with Octopress 2.
In a post at the beginning of 2015 entitled
[Octopress 3.0 Is Coming](http://octopress.org/2015/01/15/octopress-3.0-is-coming/)
he said:

> Octopress is basically some guy's Jekyll blog you can fork and modify

If like me you wanted to tweak the configuration and theme to make it your
own, while also pulling in updates, then you needed to fork the project and
regularly pull in upstream fixes.

Pretty often that meant spending time in git
conflict hell, especially since the team had been convinced
[that the Gemfile.lock shouldn't be part of the
repo](https://github.com/imathis/octopress/issues/568). Ick.

Another fundamental issue is that Octopress 2 seemed to be in a tug of war
between two paradigms: a world of systems assembled by developers from small
components that do one thing well, and a world of monolithic software
configurable by non-technical admins.

Don't get me wrong, Octopress 2 was awesome and powerful, and it worked well,
but the places which tried to be WordPress-shaped didn't seem to fit.

So Octopress 3 was going to fix all that, right? It would have some new way of
distributing updates, and a big shiny "Upgrade to 3.0" button, right?

Not quite...


## That word you keep using...

I guess I must have skipped over the massive headlines on the
`imathis/octopress` repo:

> Octopress is an obsessively designed framework for Jekyll blogging

> Octopress is Jekyll blogging at its finest

Until recently I knew [Jekyll](https://jekyllrb.com/)
only as "that thing they used for
[Obama's first fundraising
campaign](http://kylerush.net/blog/meet-the-obama-campaigns-250-million-fundraising-platform/)".
I knew they'd used it partly for performance reasons.

If I was aware my Octopress blog was using it,
then it was only an implementation detail - not
anything I needed to worry about.

Now the message has changed - the new repo proudly declares:

> Octopress 3.0 – Jekyll's Ferrari

> Octopress is an obsessively designed toolkit for writing and deploying
> Jekyll blogs


## Face it: you're a Jekyll blogger. You always were

On closer inspection, Jekyll is a Ruby gem which provides:

* A project structure for static sites
* Tools for generating and previewing site content written in [Markdown]()
and styled with [Sass]()
* An understanding of blog posts, tags, categories, permalinks etc.

I'd always assumed that the creation of posts was handled by Octopress. So
what does Octopress 2 actually add on top of this?

* A Jekyll theme
* A collection of built-in plugins (individual ruby files)
* Options in the configuration file for configuring the theme and plugins
* A collection of rake tasks for working with posts

The project structure is different to Jekyll's default, so the Rake tasks also
handle building and previewing the site.

One way to think of Octopress 2 is as a 'wrapper' around Jekyll.


## You Can't Go Home Again

So how do you upgrade? Brace yourself:

You can't.

Or at least there's no sense in which you can move all your content, templates
and styling into a new 'Octopress 3 blog'.

(Except that you _might_ be able to do that in future if you haven't made many
customisations).

Actually [you can start using Octopress 3 with your existing blog content
now](https://dgmstuart.github.io/blog/2016/01/22/migrating-from-octopress-2-to-3/), but it's not what you think.

To make sense of this, remember that Octopress 2 with all it's features is
distributed as a single project. Any individual blog might additionally add customisations:

* Additional plugins
* Changes to theme templates
* Changes to Sass/CSS

In contrast, Octopress 3 isn't one project - it's more like a namespace or
brand for a collection of tools for Jekyll blogging, all packaged as Ruby
gems:

* [`octopress`](https://github.com/octopress/octopress) adds
blog templates for pages, posts and drafts, and a
command-line interface for creating and managing the same
* [`octopress-deploy`](https://github.com/octopress/deploy) adds support for deploying Jekyll blogs
* Some of the features of Octopress like [
code-blocks](https://github.com/octopress/codeblock) and
[support for the Solarized syntax highlighting
theme](https://github.com/octopress/solarized) have been implemented as gems
which can be included as Jekyll plugins.
* [`genesis-theme`](https://github.com/octopress/genesis-theme) is a work in
progress but is planned to be a default theme for Octopress.

All of the things which are 'missing' from that list - like tools for building
and previewing the site - are features of Jekyll.

Here's the model:

|                   | Octopress 2 | Octopress 3                  |
| -                 | -           | -                            |
| Core application  | Octopress   | Jekyll                       |
| Manage posts      | Rake task   | Octopress cli command        |
| Build and preview | Rake task   | Jekyll cli command           |
| Deploy            | Rake task   | octopress-deploy cli command |

<br>

It's like an [inversion of control](): Octopress 2 used to be something you
customsed, Octopress 3 is the customisations.

