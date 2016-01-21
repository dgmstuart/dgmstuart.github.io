---
layout: post
title: "Tim Nash at WordCamp London - Beyond Unit Tests"
date: 2015-03-22 12:17:20 +0000
comments: true
categories: ["wordcamp london", testing, liveblog]
---

_I'm at [WordCamp London](http://london.wordcamp.org/2015/) - live-blogging
some of the talks_

[Tim Nash](https://twitter.com/tnash)

* Strategy for handling a tight deadline: Piles of acceptance tests
* Disagreement between different languages on what unit tests mean
* Focus on tools and concepts not methodology
* [Codececption](http://codeception.com/) - does a lot of things, mocking etc.
  php testing framework
* Acceptance testing: commands like `amOnPage`, `see`, `fillField`
* Define scenarios: login, fill in form and submit etc.
* Generates reports in: console, html, xml (Ick!)
* Can use different drivers: WebDriver (fast, headless, no js, basically does the equivalend of cURL), Selenium (Slow, but drives the actual browser: can be hard to set up), PhantomJS (headless, runs js)
* Functional testing: sits alongside acceptance testing
    * Test the interactions between units - integrations
    * Ignore UI and JS - make posts directly: don't emulate the browser - just
      call endpoints (incl. ajax) directly
    * Can also be used to test apis/rss, cli, sending email, can also test
      xml-rpc
* We'll be interacting with the database, so we need to set up and tear down
  data
    * WP-Browser handles these database interactions (i.e. you can directly
      insert data)
    * Approach for setup and teardown: you can run a shell script before and
      after a codeception test, so setup wp-cli scripts to do the database
      setup and teardown
* Re-usable code in Codeception: Step objects (collections of related steps to DRY up tests?)
  - Helpers, Modules
* Refactoring strategy: Acceptance tests allow you to refactor your unit tests
  with confidence, (which allows you to refactor your code with confidence)
* Question: How to not couple the tests to e.g. the names of field names
    * Strategy: create a page object which wraps up those IDs, so that you
      only have one point where you need to change these
    * Makes it slightly harder for non-developers to do the changes _(does it though??)_,
      but makes it much more maintainable
