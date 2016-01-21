---
layout: post
title: "Mark Jaquith at WordCamp London - Cache Money Business"
date: 2015-03-22 11:31:06 +0000
comments: true
categories: [caching, wordpress, "wordcamp london", liveblog]
---

_I'm at [WordCamp London](http://london.wordcamp.org/2015/) - live-blogging
some of the talks_

![Mark Jaquith talking caching at #wcldn - photo by @davepullig on
twitter](https://pbs.twimg.com/media/CAs0AgdWEAApTMb.jpg)

_photo of [Mark Jaquith](https://twitter.com/markjaquith) by [Dave Pullig](https://twitter.com/davepullig/)_

* Ultimately failing to cache could take your site down: run out of connections
* Caching principles:
    * Do less work - By default WP is Totally dynamic - there's a lot of
      complex stuff going on and it all gets done on every single request
    * Hold onto data as long as it makes sense to do so: this is very specific
      to each individual site (at least for certain kinds of content)
    * Make output generic: the more specific it is to a particular user, the
      less cachable it is
* Page caching:
    * Batcache - if a page gets requested more than 3 times in 3 mins it
      caches that whole page for 6 mins
    * Used by WP.com
    * Unobtrusive - only caches the most frequently requested stuff
    * low-configuration
* W3TotalCache
    * Extremely complex, lots of layers
* WPSuperCache
    * Somewhere in-between
* Varnish or Nginx
    * Cache before it hits the webserver
* CDN
    * geographically distributed caches: people access the cache closest to
      them
    * Very popular for media files, but can be used for the whole site.
    * Will obey cache headers
* Nginx Cache Purging
    * This is tricky
    * There's a third party package which can be compiled from source which
      enables this. But :-P
    * Approach: Configure a 'magic header' ;) - when it's passed with a
      request, it skips getting, but not setting, so overwrites whatever is
      currently in the cache
* Variable cache lengths + proactive cache refreshing:
    * Warm the cache (on cron) more frequently than it's getting expired, to
      make sure that your content is always cached
* Does being logged-in mean you can't cache the page?
    * It's a pain if you have 1000's of logged in users/active commenters etc.
    * Think about how the page varies based on who you are logged in as:
      toolbar, private posts - etc.
    * Choice: remove those things OR make them generic
    * Comment forms: hide them by default, show them with ajax - either when
      hitting a button, or when scrolling to the bottom of the page (!)
    * Remove toolbar for subscribers etc. - it's not very useful!
    * Remove private post fucntionality if it's not used: it makes the queries
      much more complicated anyway
    * Replace moderated comments with a generic "your comment is being
      moderated" message
* Approach if you really need to show logged-in data:
    * Use cookies: by default WP renders user-specific content using php, but
      that's very odd
    * Set JS-readable cookies and use them to populate the content
    * He's just written [Cache Buddy](https://github.com/markjaquith/cache-buddy) - a plugin to do this
* Object Caching
    * APCu, Memcache, or (recommended:) Redis
    * put them in `object-cache.php` - they need to be loaded before plugins
* Transients + Object Cache
    * For just a few values use transients - stores data in the options table
    * For 1000s of values use the object cache
    * Object cache can organise cache keys into groups
* Cache stampedes: What happens when cache expires?
    * One user triggers the remote request
    * then the next user also triggers it (before it's completed)
    * and another... STAMPEDE!
    * `tlc_transient` - handles this problem: soft expiration - data gets
      marked as expired but not deleted until the new data is available (!)
* Html fragment caching
    * e.g. A complex footer that's the same on every page
    * `CWS_Fragment_Cache`
