---
layout: post
title: "Useful rake tasks in Rails"
date: 2014-06-15 15:55:16 +0100
comments: true
categories: [Ruby, Rails]
---

When inside any directory with a [Rakefile](https://github.com/jimweirich/rake), you can bring up the list of available rake tasks with `rake -T`

In [Rails](http://rubyonrails.org/) projects everyone is used to running rake tasks related to database migration and asset management, but here are some tasks I've found useful which you might not be aware of:

* `rake notes` - Lists all the comments in your application beginning with annotations like `# TODO` and `# FIXME`
* `rake log:clear` - Clears out all of your log files (which can tend to get rather large if you're regularly developing and running tests)
* `rake routes` - Prints out all the routes defined by your `config/routes.rb` file. Very useful for debugging
* `rake tmp:clear`/`rake tmp:create` - Clear out all your tempfiles (sessions, caches etc.) / create the folders for those tempfiles.
* `rake secret` - "Generate a cryptographically secure secret key" - the recommended way to generate the secret token for your applications, but useful anywhere you need a long random key