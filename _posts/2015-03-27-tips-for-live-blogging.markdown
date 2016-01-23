---
layout: post
title: "Tips for live-blogging"
date: 2015-03-27 20:40:14 +0000
comments: true
category:
tags: [blogging, vim, octopress]
published: false
---

I've been going to a number of conferences and talks recently and I've developed a habit of taking
notes on my laptop and 'live-blogging' them: getting them up online as quickly as possible
after the talk finishes. You can see some examples
[here](/blog/categories/liveblog/)
([source](https://github.com/dgmstuart/dgmstuart.github.io/tree/source/source/_posts)).

## Workflow
I take my notes in [Markdown](https://daringfireball.net/projects/markdown/syntax)
using [vim](http://www.vim.org/) and publish them with [Octopress](http://octopress.org/)
on this blog, but the basic principles should apply regardless of the tools used

1. Open a new post in vim with `newpost "My Post title"`
2. In another terminal window, run `rake isolate[unique words in title]`,
   followed by `rake preview`
3. Start typing, occasionally writing (`:w`) and checking the post in a
   browser window (http://localhost:4000) to make sure the formatting is
   correct
4. As soon as the talk is finished, type `rgd` to publish the post
5. Tweet about the post, tagged with the event's hashtag
6. Find a photo which someone has tweeted of the talk (usually on instagram)
   and add it with `![Speaker speaking at event](Link to photo)`, as well as
   crediting the photographer and linking to their twitter
7. Deploy again (`rgd`) and tweet at the photographer telling them I've done
   so and asking if that's OK
8. When I get a chance, commit and push the source

_For a detailed description of all those commands see my previous post on
[tools for fast blogging in Octopress]_

Some things to note:
* I publish and tweet the post before I've finished doing the more time-consuming
  tasks like finding and publishing photos
* If I add in any links, I'll try and do that on the fly, but it can wait until
  that first tweet goes out.

## Publish and be damned
Notice there's no mention in any of the above of editing. That's because I don't do any.

OK, so I might add in a couple of links if I think it's important, I know that if I hold back
publishing until I'd corrected all my mistakes, explained all my ambiguities
and added in links to all the references, I'll never publish anything at all. I
feel like it's much more valuable for those notes to be up as soon as
possible, than for them to be a nicely formed, well-presented narrative.

On top of that, I believe the main benefit is as a form of
[active learning](https://www.linkedin.com/pulse/20130702175823-659753-if-you-aren-t-taking-notes-you-aren-t-learning):
the value is in the process of taking notes, not in the end result.

Of course it's good to set expectations: I always tweet as "I've blogged my notes", rather
than claiming that I've crafted a well thought-through post.
