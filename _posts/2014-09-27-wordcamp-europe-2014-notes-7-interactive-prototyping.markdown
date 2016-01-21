---
layout: post
title: "WordCamp Europe 2014 notes - 1.7: Interactive prototyping"
date: 2014-09-27 16:40:38 +0300
comments: true
categories: [WordCampEurope, CSS, Prototyping, liveblog]
---

_I'm at [WordCamp Europe](http://2014.europe.wordcamp.org/) in Sofia - taking rough notes on some of the talks_

Karin Christen http://2014.europe.wordcamp.org/session/karin-christen/

![sketchnotes by @studionetting](http://photos-e.ak.instagram.com/hphotos-ak-xaf1/10665594_1562716813952100_214832350_n.jpg)

_Awesome sketchnotes by [@studionetting](http://instagram.com/p/tc-b8atkPN/)_

* Used dynamic style guide - one page in the prototype
* Started in the browser right from the beginning - speed benefits but also:
  * responsiveness from the beginning
  * easy reference for the existing styles
  * visual specification - communication tool
* Prototypes = a basis for a specification for user testing
* php doesn't work for dynamic prototypes - just html/css
* Made a prototype Generator (in js?)
  * Installs quickly
  * Dynamic content without php (how??)
  * Benefit: less noise from php
* http://vuejs.org/
  * Adds attributes which can be added to markup which can e.g. optionally show and hide elements based on url query params
* Closing comment: "The modern prototype is a living playground, not a project milestone"

* Audience question - if you're using wp - why don't you just jump into wp? Why go to the effort of generating content in JS?
  * Answer: the project example was for a complex back-end which didn't exist yet. With WP their process is slightly different: start off designing in the browser (test there?), then port over to WP (once it's validated?)

* My question - how do you deal with responsive elements when you don't know what the context will be: media queries only work with the whole size of the page.
  * Answer: we design in context
