---
layout: post
title: 'Gotcha: Jekyll post generated with wrong date'
date: 2022-02-19 11:53 +0100
comments: true
categories: [jekyll]
tags: [jekyll, timezones]
---

It is said that most of the hard problems of computer science have been
solved, but two remain: cache invalidation, naming, and off-by-one errors.

[Timezones](https://thoughtbot.com/blog/its-about-time-zones) may in theory be
a solved problem ("'just' always work in UTC!"), but it's one which still comes up
annoyingly often.

### Building locally

My previous deployment flow for this blog you're reading right now was to
build it locally with `jekyll build`, and use
[Octopress deploy](https://github.com/octopress/deploy)
to deploy it to a separate repo.

While this was in place, the previous blog post was correctly generated with a
date of the 18th of February:

- <https://dgmstuart.github.io/blog/2022/02/18/building-a-bingo-app-in-react/>

### Building on CI

When I switched to using a
[Github action](https://github.com/dgmstuart/dgmstuart.github.io/blob/main/.github/workflows/github-pages.yml)
to deploy automatically on push, the post started getting generated with a
date of the 17th:

- <https://dgmstuart.github.io/blog/2022/02/17/building-a-bingo-app-in-react/>

This was pretty frustrating: the filename said the 18th:

    2022-02-18-building-a-bingo-app-in-react.markdown

...and the date in the Jekyll
[front-matter](https://jekyllrb.com/docs/front-matter/),
(which I had set manually to be at midnight) was the 18th:

    ---
    layout: post
    title: "\"Somebody is upside down!\": building a bingo app in React"
    date: 2022-02-18 00:00:00 +0100
    comments: true
    categories: [reactjs]
    tags: [reactjs, react]
    ---

But observant readers will have spotted the problem:

Previously I was building the site on my local machine, which is in Sweden,
where `2022-02-18 00:00:00 +0100` is midnight on the 18th.

But the Github action is running in UTC (as all good servers do), and in UTC
`2022-02-18 00:00:00 +0100` is 23:00 on the 17th.

Changing the timestamp to midday fixes the issue: `2022-02-18 00:12:00 +0100`.

## Conclusions

I should know better: when there's a problem involving differences between CI
and local, _particularly_ when the differences are to do with times, timezones
are a very likely culprit.

I guess there are two morals to this story:

1. don't write blog posts close to midnight
2. don't arbitrarily set timestamps close to midnight

This sort of problem was less common when I lived in the UK (except in summer).
