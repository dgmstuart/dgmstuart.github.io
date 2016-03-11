---
layout: post
title: "Bath Ruby 1: Xavier Riley - Rocking out in Ruby, a playful
introduction to Sonic Pi"
date: 2016-03-11T09:39:26+00:00
comments: true
category: liveblog
tags: [ruby, bathruby]
---

* The first time you enjoyed writing code: it's an important feeling - an
  important step on the way to becoming a programmer
* [Sonic Pi](http://sonic-pi.net/) - cross-platform and preinstalled on every raspberry pi
* API:
  * Synths
  * Samples
  * Effects
* Uses Ruby internally - responds to RUBY_VERSION
<iframe src="https://vine.co/v/iHDKUtIlHTL/embed/simple" width="600"
height="600" frameborder="0"></iframe><script
src="https://platform.vine.co/static/scripts/embed.js"></script>
* Zero set-up: getting started is very quick, and the feedback loop is very
small
* Gold standard:
  * "If a 10 year old can't use it and understand it, it's not going in the API"
* Default - all notes happen at time zero - to make a scale, put sleeps
between the notes
* The `play` command takes either a number (pitch) or a symbol (note name)
* Synths - just a symbol saying what synth the following notes should be
played through
* Samples: Can be slowed down and sped up with a `rate` argument
* `use_sample_bpm` - changes `sleep 1` to be the length of one beat - means
you don't have to specify the length of the sleep
* Live loops: continually plays and updates in realtime
* `play(scale :c, :minor_pentatonic).choose` - random notes from a scale
* Multiple `live_loop`s in a program - each one is a thread and they're
in time with each other (!)
* He learned to use it while studying for a music degree :)
* 3000 words of tutorial - step by step through the whole thing
* A 'ring' is an array which loops around.
* A slice is a section of a sample - from a start time to an end time
* Played the Mario theme with NES synths :D
* Markov chains: train on some real music and it can generate similar music :D
* There's no concept of time in webservers - speed is all that matters. Music
is a very different challenge. It's a different interaction with code. You can
edit code in a way similar to playing a musical instrument: responding to
what's happening in a direct way.

