---
layout: post
title: "WordCamp Europe 2014 notes - 1.3: Styling the WordPress admin"
date: 2014-09-27 11:32:55 +0300
comments: true
categories: [WordPress, WordCampEurope, liveblog]
---

_I'm at [WordCamp Europe](http://2014.europe.wordcamp.org/) in Sofia - taking rough notes on some of the talks_

Konstantin Dankov http://2014.europe.wordcamp.org/session/konstantin-dankov/

![sketchnotes by @studionetting](http://photos-a.ak.instagram.com/hphotos-ak-xfp1/1742413_291384914397664_162274686_n.jpg)

_Awesome sketchnotes by [@studionetting](http://instagram.com/p/tcl0LKNkJp)_

* Why style the admin?
  * make things simpler for the user
  * save time by optimising workflows - supporting power users - removing unnecessary steps: e.g. jump straight to a particular page.
  * Branding - sometimes larger organisations would like their own branding inside
    * Important to not go overboard! Use a little to do a lot.

* Why NOT to do it
  * There's a large cost to supporting it:
    * There's a lot of it!
  * Clashes with plugin styles: can cause plugins to break

* Consider the experience of editors vs admins

* Adding a custom post type
  * Defaults (all overridable when registering the post type)
    * It goes to the bottom of the list
    * It has the same icon as other post

* Using filters the order of menu items can be managed in a more coherent way: explicitly naming the order rather than using index numbers.

* Preference: When the custom post type is specific to that project then this menu-modifying code should be in the theme, along with the post type itself: moving it out into a plugin just adds complexity.

* Removing the dashboard to jump straight to the posts: useful if your primary thing is generating new content

* There's a lot of complexity in the WP admin css: responsive styles, Right->Left styles. Making modifications are a lot more work than they look




