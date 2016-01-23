---
layout: post
title: "WordCamp Europe 2014 notes - 1.5: Postmodern Wordpress"
date: 2014-09-27 14:32:07 +0300
comments: true
category: liveblog
tags: [wordpress, wordcampeurope]
---

_I'm at [WordCamp Europe](http://2014.europe.wordcamp.org/) in Sofia - taking rough notes on some of the talks_

Andrew Nacin: http://2014.europe.wordcamp.org/session/andrew-nacin/

* WP enables non-devs to do dev
  * Low barrier to entry
  * gateway drug
* WP is increasing in complexity
  * Part of it is about migrating functionality into js
* The world of web dev is getting more complicated: html5, css3 - (Raises barrier to entry??)
* "We're really good at managing backwards compatibility"
* Dev focus is on improving the UX: source of complexity
* Improving docs for devs is important - experienced devs forget that it's an issue.
* Complexity needs to not come at the cost of the philosophies: trivial setup etc.
* ...balanced against drawing the experienced
* "you know what? I'll build this site in WP, because WP doesn't make me want to tear my hair out any more"
* WP needs to be more consistent: decrease the time spent searching through docs/trying to work out what an argument means
  * inconsistencies trip people up
  * struggle - can't break backwards-compatability
* Objects should (/will have?) implementations of http://php.net/manual/en/class.jsonserializable.php
* A lot of people don't actually know they're writing php (!)
* “ I really can’t wait to have [these features] because it’s, like, *Sanity* and that’s a good thing to have”
* WP uses objects but in a functional way - actually going OO wouldn't make sense (?)
* http://backpress.org/
* WP should only load the necessary files - not 120.
* Also you should be able to pull individual files out and use them in non-wp projects.
  * The way you do that is by decoupling -> this also makes debugging easier
* You shouldn't need to understand what's going on under the hood in WP in order to build stuff which works.

* Audience observation - Automattic - preferred to use REST API to WP function calls (?) because the api is more consistent

* Audience qu - I'm missing a debugger
  * Use an IDE. We could be doing more to make debugging easier, but there are a bunch of tools out there which help.
* Posts might end up with relationships. Terms will probably end up with metadata
  * Post relationships now (via a plugin): https://wordpress.org/plugins/posts-to-posts/

* "It's not a matter of doing it, it's a matter of planning it out so that we don't have to Re-do it in a year"
