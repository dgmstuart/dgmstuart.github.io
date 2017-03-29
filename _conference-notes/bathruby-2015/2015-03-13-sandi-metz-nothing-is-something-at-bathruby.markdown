---
layout: post
title: "Sandi Metz: Nothing is Something - at BathRuby"
date: 2015-03-13 16:38:05 +0000
comments: true
category: liveblog
tags: [bathruby]
---

_I'm at [Bath Ruby 2015](http://2015.bathruby.org/), live blogging some of the
talks_

* All of her teaching ideas have a single underlying principle
* Learned OO from Smalltalk - massive influence on the way she thinks about
  objects
* `+` is just a method on `Fixnum`
* Ruby's `if` is effectively a typecheck (a check of whether an object is a
  member of `NilClass`/`Trueclass` ??)
  * `if` is very prodedural (not OO)
* Sometimes `nil` means nothing - so just remove nils from your results (e.g.
  `compact` the array
* `try` can effectively be a typecheck (for `NilClass`)
* Conditions breed: if you have one you'll get more. Making a change around
  them in one place may involve making changes in lots of places ("shotgun
  surgery")
* If you return nil then calls to that object can return objects which respond
  to different Apis: The object's or NilClass's. Solution: Null Object Pattern
  (#ftw)
* Null Object is for when Nothing is actually a thing - for when you need to
  talk to it
* Null Object is a new dependency, so wrap it in a new class which returns
  either an instance of your Object, or the Null Object, then pass this new
  class around
* Null Object is a concrete implementation of a more general idea
* Single responsibilities allow you to override specific behaviours when
  inheriting
* Extending behaviour by inheritance makes it impossible to share multiple
  behaviours
* Inheritance is for specialisation (not for ??)
  * There's no such thing as just one specialisation...
* Approach - Composition and dependecy injection:
  1. make the original behaviour and the new behaviour look the same - this
     highlights the way they're different
  2. Give the new behaviour a name - this helps work out what's being applied
     to the original object
  3. Inject an object which represents the behaviour

