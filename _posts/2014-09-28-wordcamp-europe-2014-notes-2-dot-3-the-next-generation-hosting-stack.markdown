---
layout: post
title: "WordCamp Europe 2014 notes - 2.3: The Next Generation Hosting Stack"
date: 2014-09-28 11:33:02 +0300
comments: true
categories: [WordPress, PHP, WordCampEurope, liveblog]
---

_I'm at [WordCamp Europe](http://2014.europe.wordcamp.org/) in Sofia - taking rough notes on some of the talks_

Mark Jaquith (@markjaquith) http://2014.europe.wordcamp.org/session/mark-jaquith/

* Why is performance important?
  * It's not about overall speed - it's about the frustration of being interrupted
  * Everyone hates Lag
* WP has a bad rep for speed
  * no matter what wp core do, WP DB's are getting bigger, lots of functionality, lots of plugins
* WP is dynamic - everything is built on the fly
  * Why don't we just generate static files? - No persistent caching in core. Pages are so dynamic: search results etc. Only a small number of pages can be cached
* For the purposes of this talk: speed = speed of download of just the html page (not css or other assets)
* 3+ seconds = EMERGENCY
* 1-3 seconds = Slow
* 100-250ms = fast
* <100ms = instant
* Many hosts don't help with static caching
* Virtual server hosting - what to look for
  * Are they going to be around for a while?
  * What's their raw performance - lots of people have done hardcore speed tests - easily googleable
  * Value for money
  * Tool support - provisioning scripts, auto backup
  * Recommended: Linode, Digital Ocean

## Web Server

* "Nginx is the best thing to happen to web servers ever"
* Apache approach - php is baked in. That means there's an overhead for each request
* Nginx approach - can't have php baked in. PHP-FPM runs php
* PHP 5.5 has significant performance improvements over previous versions
* HHVM - facebook project: transcodes php into machine code on the fly, with caching = crazy fast
  * Might not be production stable - need monitoring to catch when it crashes

## Database

* MySQL is not the only option: MariaDB, Percona
* HyperDB - project for providing a layer to manage lots of database instances

## Caching

### Page caching:

* Questions - who, what, how long. Answers: Anonymous???, front of site, minutes, maybe hours???.
* Nginx has a cache
* Varnish is another option, but is another moving part
* Plugins are tightly integrated with WP, but run in php so are much slower

### Object caching:
* Recommend using Redis (Pantheon uses it)
* Memcached - used to be a recommendation. Restarting flushes the data - can cause massive spikes in load
* Mark's wrappers:
  * TLC Tranients http://bitly/tlc-transients
  * Fragment caching: http://bitly/fragment-cache

### The next generation stack
![The next generation WordPress hosting stack](https://pbs.twimg.com/media/Bym-26pCcAAyPVd.jpg:large)

* Audience qu Hyper db - works with MariaDB & Percona?
  * AFAIK yes

* My qu - can you make core use Redis for your object cache or is that just for your own caching
  * Yes - you configure an object cache and core will use it to cache posts, taxonomies, a bunch of other things.

* Audience qu: What do you use for performance monitoring
  * NewRelic: very expensive, but gives you amazing insights

*
