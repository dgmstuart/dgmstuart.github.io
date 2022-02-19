---
layout: post
title: "Migrating From Octopress to Jekyll::Compose"
date: 2022-02-20T20:57:37+01:00
---

I recently removed all [Octopress](http://octopress.org/) dependencies in this
blog in favour of native features of the underlying [Jekyll](https://jekyllrb.com/)
static site generator.

I'd like to share that experience with you, but first I want to share a bit of
the history (from the perspective of myself as a user of the tool, so please
take everything with a pinch of salt.

PREVIOUS POST

stuck on Jekyll 3

```
octopress was resolved to 3.1.0, which depends on
      octopress-escape-code (~> 2.0) was resolved to 2.1.1, which depends on
        jekyll (~> 3.0)
```


Deprecation warnings

```sh
Deprecation: posts.empty? should be changed to posts.docs.empty?.
             Called by ["/Users/dgmstuart/.asdf/installs/ruby/2.7.2/lib/ruby/gems/2.7.0/gems/jekyll-3.9.1/lib/jekyll/collection.rb:39:in `method_missing'"].
Deprecation: posts.select should be changed to posts.docs.select.
             Called by ["/Users/dgmstuart/.asdf/installs/ruby/2.7.2/lib/ruby/gems/2.7.0/gems/jekyll-3.9.1/lib/jekyll/collection.rb:39:in `method_missing'"].
```


Step 1 remove dependencies on Octopress plugins

I have three octopress plugins:

  gem 'octopress-codeblock'
  gem 'octopress-image-tag'
  gem 'octopress-solarized'

Codeblock

doesn't support filenames

I just inline them

After fixing all tags:

Unknown tag 'css_asset_tag' included  (Liquid::SyntaxError)

remove

```liquid
\{    % css_asset_tag %}
```



Now they look wonky - indented

This is because they display as a `<figure>` tag and (at least in Chrome) the
default styles include a left-margin:

figure {
  display: block;
  margin-block-start: 1em;
  margin-block-end: 1em;
  margin-inline-start: 40px;
  margin-inline-end: 40px;
}

...but first we need to fix loading our css at all:octopress did that for us

https://jekyllrb.com/docs/step-by-step/07-assets/

move css under /assets

create styles.scss

Import all files there

import pixyll into styles
remove pixyll front matter

modify pixyll code so that we can override it with rouge

rougify style base16.solarized.dark > _sass/rouge.scss

