---
layout: post
title: "Ben Orenstein: live coding in the Bath at BathRuby"
date: 2015-03-13 10:34:52 +0000
comments: true
category: liveblog
tags: [ruby, bathruby]
---
_I'm at [Bath Ruby 2015](http://2015.bathruby.org/), live blogging some of the
talks_

* Live coding!
* Code for sending mail to a list of recipients based on a csv
* Problems:
  * Will fail if one of the recipients fails to get parsed
  * CSV should get injected as a dependency rather than bring parsed in this
    class.
  * Class has multiple responsibilities: parsing and mailing
* TDD: Should take very little time and effort to run tests (his are two
  keystrokes and no time)
* an 'extract class' refactoring:
  1. add the class we  want in the initialiser
  1. create an empty spec
  1. create the class

* `CSV::strip_heredoc` - Look up
* Don't write specs for private methods - test them implicitly through the
  public methods
* Spec style for parsing output:
  1. create a csv string
  2. create an output hash
  3. expect parsing the csv string to equal the output hash
* TDD approach - do only the smalles thing to get the message to change: e.g.
  create initializer with no args, then add one arg, then start to define
  behaviour
* Now actually use the parser.
* Tests green = OVERWHELMING INSTINCT TO COMMIT!
* "Component test" = integration test just covering a selection of components
  (?)
* Test refactoring: extract methods! (e.g. to do repeated stubbing and other
  setup)
  * could use `before`, but tends to do this later or not at all. Makes specs
    harder to read and maintain (?): each spec is different. Extracting method is definitely the first
    step.
* "We make a mess first, then we clean it up"
* Invert control: it's odd that we pass the csv into the Invitations class
  * Instead, "build dependencies at higher level and pass it down to lower
    levels" (??)
  * Instead of passing the csv to invitations and creating a parser based on
    it, let's send invitations to the parser (!)
* Inverting control benefits:
  * Easier to follow Open/Closed principle
  * Shorter specs
* Question "The specs passed from the beginning, so weren't you done?"
  * Answer: Refactoring makes more flexible maintainable code
