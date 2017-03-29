---
layout: post
title: "WordCamp London - Bruce Lawson - Responsive Images"
date: 2015-03-21 15:18:00 +0000
comments: true
category: liveblog
tags: ["wordcamp london"]
---

_I'm at [WordCamp London](http://london.wordcamp.org/2015/) - live-blogging
some of the talks_

[Bruce Lawson](http://www.brucelawson.co.uk/)

_EDIT: Bruce has added some corrections and clarifications in the comments
below_

* Responsiveness includes speed of loading
*   0.5 second delay is a 20% drop in traffic
* Images are a major cause of slowing down pages
* Av web page is 1.9MB - of which 1.2MB are images
* Number of images loaded is static over time, but the size of images
  increases significantly over time
* How can I send huge images to retina devices, but smaller images for
  rendering on smaller screens? Answer: HARD.
* First try: use css to swap in images based on page width:
    * Fail: loads the retina image THEN the smaller image - i.e. downloads
      both - the opposite of what we want
    * This is because CSS and JS get applied to the DOM, so the DOM needs to
      get loaded first (?)
    * Browsers can create the DOM tree in whatever way they like
    * Because of preloading ("the single greatest performance improvement
      that browsers have ever made") The whole of the source is read before the DOM tree is loaded - as soon
      as an image is spotted in the DOM, a request is sent off to fetch that
      image (i.e. before the DOM tree gets constructed)
    * therefore doing things in the  CSS is too late
* Therefore it needs to be the markup - Media Queries have been around a long
  time
* Respimg - responsive images community group
* First time a group of web developers wrote part of the standard and got it
  into the spec
* Now in Opera and Chrome and soon to be in Webkit
* First use case - optimise for high dpi screeens:
    * srcset attribute: specify images for particular pixel densities: a
      candidate list which the browser can use to select an optimal image
    * The browser gets to choose - this is so that e.g. the user can choose what
      sort of image quality they wnat to have

![@Brucel talking responsive images](https://igcdn-photos-b-a.akamaihd.net/hphotos-ak-xaf1/t51.2885-15/11023186_1799356763623801_1327635725_n.jpg)

* Second use case - stretchy images
    * Can be done straightforwardly with css, but involves sending massive
      images down the wire and putting a lot of CPU load on the browser
    * Slows things down for the user!
    * solution: w descriptor - use in src set to tell the browser which image
      is more suitable for a particular page width
* All this requires a lot of cruft to add in all the clauses to make this work, but
  there's a [plugin from Respimg](https://wordpress.org/plugins/ricg-responsive-images/) which does it for you
* Third use case - new image formats
    * JPG/PNG/GIF are ubiquitous, but there are more modern, better
      compression alternatives: smaller files, better quality - e.g. WebP
    * Traditionally the logic is you can't use this in the wild because only
      Chrome and Opera support it
    * with the `<picture>` element you can supply different image formats with
      fallbacks - so e.g. you can use WebP and fall back to img (actually
      browsers will force you to do progressive enhancement (supply an image)
* Last use case - art direction
    * Choose a different aspect of the image to display on different devices:
      zoom in on a different bit of the image for different viewport sizes
    * Use media queries within the `<picture>` element to display different
      versions of an image at different widths
* Release dates:
    * in OPera and Chrome now
    * FF in may
    * IE "under consideration" ...
    * Safari already implement `srcset` - havent talked about `<picture>`
* YOU CAN USE THIS NOW
   * It's designed to force progressive enhancement - nobody gets a worse
     experience than they do now
