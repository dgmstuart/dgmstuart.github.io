---
layout: post
title: "Octopress: A Eulogy"
date: 2022-02-21T00:08:00+01:00
comments: true
categories: [octopress]
tags: [octopress, jekyll]
---

I recently removed all [Octopress](http://octopress.org/) dependencies in this
blog in favour of native features of the underlying [Jekyll](https://jekyllrb.com/)
static site generator.

I'd like to share that experience with you, but first I want to share a bit of
the history (from the perspective of myself as a user of the tool, so please
take everything with a pinch of salt)

I first want to say that I really enjoyed the experience of using Octopress.
It was my first experience of a static site generator, and it allowed me to
use Vim for blogging, to create and preview posts while offline, and to
[liveblog from a bunch of conferences](https://dgmstuart.github.io/conference-notes/).

It feels like the motivation behind it was a good one: create a
markdown/static site-based blogging tool that was more fully featured than
other things available at the time.

### Version 2: "Framework"

I started using Octopress at version 2 in 2016, which described itself as:
["an obsessively designed framework for Jekyll blogging"](https://github.com/imathis/octopress)

...but as creator
[Brandon Mathis](https://github.com/imathis) would later write, it could also
be described as:
["basically some guy's Jekyll blog you can fork and modify"](http://octopress.org/2015/01/15/octopress-3.0-is-coming/),
which meant that getting updates and fixes could be pretty painful.

It came with a hardcoded list of plugins to extend Jekyll's functionality
with a bunch of custom tags that allowed more control over how things like
[images](http://octopress.org/docs/plugins/image-tag/) and
[code blocks](http://octopress.org/docs/plugins/codeblock/)
were displayed.

In hindsight it feels like these trying to find a sweet spot between markdown and HTML,
maybe similar to what
[WordPress shortcodes](https://wordpress.com/support/shortcodes/) do.

Automation of things like creating posts and running the server were wrapped
in Rake tasks, which works, but could be a bit fiddly when passing arguments
like a post name.

For all it's issues, it was widely loved and widely used for programming blogs
including those of
[Dave Thomas](https://pragdave.me/) and
[David Chelimsky](http://blog.davidchelimsky.net/). A lot of these blogs
sidestepped some of the issues by just using "the default theme" - ie.
Brandon's Jekyll blog.

### Version 3: "Toolkit"

Version 3 turned things inside-out: it went from being a wrapper around Jekyll
(largely insulated the user from it), to being a set of jekyll plugins to
improve the experience of using Jekyll as a blog.

The main [octopress/octopress](https://github.com/octopress/octopress) gem was
now "just" a command line tool, for initialising a new blog, creating posts,
deploying etc. No more Rake tasks! This was actually great.

The code for creating the image and code block tags etc were all moved to
individual plugins, as were a number of utilities like
[octopress/ink](https://github.com/octopress/ink) and
[octopress/filters](https://github.com/octopress/filters),
which (and this is a guess) worked around some of the limitations of Jekyll to
improve the experience of developing plugins for Jekyll.

### So you think you're an Octopress blogger?

From an architectural design perspective, this was very much the right way to
go: moving from a monolithic framework tightly coupled to the core engine, to a
collection of opt-in single-responsibility tools, feels like a great design
choice: an
[inversion of control](https://en.wikipedia.org/wiki/Inversion_of_control).

However, it was pretty confusing for a lot of Octopress users who (myself
included) didn't really appreciate that the core templating and building of
the site was actually Jekyll.

A common question when Octopress 3 was announced was "how do I upgrade?", to
which the realistic answer is "you can't: you need to
_migrate_": generate a new Jekyll blog (ideally with the 3.0 version of
`octopress new`, move over your content, and then try to work out how to
migrate any customisations you'd made yourself.

It didn't help that (as far as I know?) there was never an official
upgrade/migration guide (I published
[my own](https://dgmstuart.github.io/blog/2016/01/22/migrating-from-octopress-2-to-3/)),
and that caused some bad feelings among people who didn't appreciate that Open
Source Is Hard.

### Fading into being unmaintained

While Octopress 2 had been a single application, Octopress 3 was more of a
concept: while the core tools were ready and some of the plugins were ready
before others, and maybe some were never completed?

The last post on the Octopress blog was
[the announcement blog for Octopress 3](http://octopress.org/2015/01/15/octopress-3.0-is-coming/)
at the beginning of 2015

The last commit to the main repo was at the beginning of 2016, and most of the
other repos are older than that.

It seems like they just quietly became unmaintained. Which is not surprising:
it seems like a majority of the code on this huge collection of repos was
written by one person, and it's the kind of software that a lot of people can
end up relying on to some extent, but cannot reasonably be monetized.

### Holding on

It's been possible to continue using Octopress for quite a long time. I have
a fork which only needed a couple of commits on top of it to keep the toolset
working.

Unfortunately continuing to use it meant being stuck on Jekyll 3, and I like
being up to date with my software where possible, so Octopress had to go.

Here's how things are managed now, compared to with the two versions of
Octopress I used:

|                   | Octopress 2 | Octopress 3                  | Jekyll::Compose |
| -                 | -           | -                            | - |
| Core application  | Octopress   | Jekyll                       | Jekyll |
| Manage posts      | Rake task   | Octopress cli command        | Jekyll cli command |
| Build and preview | Rake task   | Jekyll cli command           | Jekyll cli command |
| Deploy            | Rake task   | octopress-deploy cli command | Github Actions |


## A couple of design reflections:

### Did I really need it?

One thing I noticed when removing the other plugins was how (at least today)
they're pretty redundant:

At the time, I didn't understand Jekyll, so it didn't occur to me that you can
of course create basic image tags and code blocks directly in markdown and
that Jekyll will build pages based on that markdown.

Sure, the Octopress tags were more configurable, but for the most part I
wasn't actually using any of that configuration: I thought that was just the
way that you add images etc.

The Octopress 3 core command line tool was super useful though.

### If you can't beat them, join them

Some parts of the Octopress 3 collection of tools looked like they were
features which really should be in Jekyll core. I wonder what the boundary
is/should be between submitting a PR to the repo vs adding a plugin.

Speaking of the command line tools,
[Jekyll::Compose](https://github.com/jekyll/jekyll-compose)
seems to have actually been around longer than Octopress 3, and seems to have
been started by probably the secondary contributor to Octopress?

### Avoid max version constraints?

One of the issues which prevented updates of Octopress dependencies in the
past was that the Octopress gemspec had an upper version constraint on the
[`jekyll/mercenary`](https://github.com/jekyll/mercenary) gem.

I've never really understood why gem maintainers do this: it feels like
there's a pre-emptive assumption that future versions which haven't been
released yet will be incompatible? I'm not sure that's true. Without an upper
restriction surely the flow for an end user is either:

1. new version of that dependency gets released
2. consumers of the gem upgrade that dependency and everything works fine

Or

1. new version of that dependency gets released
2. consumers of the gem upgrade that dependency and find that their app breaks
3. consumers lock the version of the problematic gem in their project
   Gemfiles and submit an issue or PR to try and get the issue fixed.

Either way it feels like less work for the maintainer, since there's less
pressure to cut a new gem with an updated version constraint.

### Was it Rails-ish?

Clearly Octopress was in some ways inspired by WordPress (the clue's in the
name) but I also wonder how much of the approach that Octopress took was
influenced by Ruby on Rails.

This is pure speculation (and most likely biased by me having a lot of
experience with Rails), but here are some obvious concepts like
[octopress/asset-pipeline](https://github.com/octopress/asset-pipeline)
which have a clear association, and maybe
[octopress/hooks](https://github.com/octopress/hooks)
recalls Rails's various life cycle callbacks (though this is more of a
stretch).

But also the approach in Octopress 2 of wrapping a bunch of tools into one
package recalls somewhat the Rails concept
([as expressed by DHH](https://www.youtube.com/watch?v=KJVTM7mE1Cc)) of a
backpack containing everything you might need.

And this reminds me of something I see in the wild quite a lot, and certainly
that I've been guilty of myself in the past: inappropriately following
patterns seen in Rails framework code, on the basis that "Rails is great and
therefore this pattern must be great".
