---
layout: post
title: "Creating a new app on heroku really is stupidly easy"
date: 2014-08-15 13:49:11 +0100
comments: true
categories: [ruby, rails]
tags: [ruby, rails, heroku]
---

    heroku apps:create swingoutlondon2 --region eu
    git push heroku master
    heroku run rake db:migrate

Done *. https://swingoutlondon2.herokuapp.com exists.

I know it's been around for ages, but it really is amazing how simple it is.

_* Actually not quite done - I still needed to:_

  * _set up my [secret tokens](https://devcenter.heroku.com/changelog-items/426)_
  * _jump through a couple of hoops to get Devise's key to be recognised because [Heroku doesn't have access to the Rails environment when deploying](https://stackoverflow.com/questions/19832218/failing-to-find-envsecret-key-in-devise-setup-on-heroku#comment35154876_19833145)_
  * _[tweak my production.rb](http://stackoverflow.com/a/17082720/1035431) to get it to compile assets_

_But whatever. Hush._
