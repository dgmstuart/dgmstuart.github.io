---
layout: post
title: "WordCamp Europe 2014 notes - 2.4: Code Deodorant"
date: 2014-09-28 12:27:07 +0300
comments: true
category: liveblog
tags: [wordpress, php, wordcampeurope]
---
_I'm at [WordCamp Europe](http://2014.europe.wordcamp.org/) in Sofia - taking rough notes on some of the talks_

Tom Nowell (cftp) http://2014.europe.wordcamp.org/session/tom-nowell/

* Now when running vanilla WP without plugins you shouldn't see any warnings, so all warnings are things you should pay attention to
* '@' swallows errors - hides them.
* [pre_get_posts action](http://codex.wordpress.org/Plugin_API/Action_Reference/pre_get_posts) - useful for removing complexity: modifying queries before they get run
* Law of Demeter "if you're talking to something, only talk to the things directly adjacent to you"
* No global variables - random ordering of tests can help to show up issues.
* Tools for code quality:
  * php syntax checking - if your editor isn't doing this you're doing it wrong. PHP Storm does this
  * [php code sniffer](https://github.com/squizlabs/PHP_CodeSniffer) - give it coding standards - it'll check them
  * [php Metrics](https://github.com/Halleck45/PhpMetrics)
  * phpLOC - tells you how many lines, classes, namespaces, measure of code complexity
  * [php Mess detector](https://github.com/phpmd/phpmd) - fuzzy matching: helps detect typos and join up e.g. misspelled variables which might be the same thing
  * NPath & Cyclomatic complexity - Cycl = number of points at which code may diverge, NPath is number of execution paths through the code.
    * NPath complexity is roughly equivalent to the number of unit tests that you need to have to fully test the function
* Lots of WP functions have incredibly large npath complexity - WP_Query::get_posts is insanely large. Even larger in WP 4.0
  * http://www.tomjn.com/390/wp_queryget_posts/
