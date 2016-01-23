---
layout: post
title: "Unboxed Event notes: \"Unstick your digital products\""
date: 2015-03-25 09:55:22 +0000
comments: true
category: liveblog
tags: [agile]
---

_I'm at a [speaker session from Unboxed consulting](https://www.eventbrite.co.uk/e/unstick-your-digital-products-rapidly-progress-a-complex-product-or-portfolio-of-stalled-products-tickets-15872783924)_

## [Dave Evans](https://www.linkedin.com/pub/dave-evans/a/311/919) - Product Manager (E-commerce) at Macmillan Publishing Group
### “Trust me, I’m a product manager”: case study from publishing
* Sales thinking: identify influencers, budget holders etc.
* Commercial products: it's all about profit-and-loss. Non-commercial products
  need different KPIs
* Product manager != project manager. Responsible for:
    * Backlog, vision statement
    * Roadmap: what questions do we need to answer in the next windows of time
* "You need to go rogue" - prove concepts on your own without a team
* 404 test = e.g. buy now button - show it to a % of users, does nothing
* Concierge test = build front-end, but handle the back-end manually
* Everything in the backlog should be in line with the vision statement
* Question for unpicking feature requests which you think might not actually
  be about solving a problem: "What happens if you _can't_ build it?"


## [Glyn Parry](https://twitter.com/g_parry24/) – SH:24 (NHS collaboration)
### How designing a new experience in the NHS helped to unblock and unleash new potential
* His experience: People in localgov/NHS aren't familiar with agile
* Just because they don't think of thinks in agile terminology doesn't mean
  they don't have an agile mindset
* Sexual health services in Lambeth&Southwark are very overstretched, and in
  rural areas, round-trip times to visit a clinic might be very long
* Online STI kits - convenient but expensive
* New service: free, user-focussed
* User journeys illustrated with comic-strip style graphics:
  ![@g_parry_24 talking about SH:24 approach](https://igcdn-photos-e-a.akamaihd.net/hphotos-ak-xaf1/t51.2885-15/11055891_730864497033484_1856034297_n.jpg)
* Identify people in the organisation who have an agile mindset and are quite
  influential
* Approach: agile prototyping, building a service piece-by-piece with users
* Developing personas
* It's a digital project, but only 25% of the service being provided is
  digital - needs to be recognised
* Started with the basic GDS form
* Prototyping the kits that people recieve - cardboard and post-its
  * Started out trying to design engaging packaging for the test kits
  * through user testing: learned that people don't want this - they like a
    really simple approach
* Need to think about the user needs of the people receiving the tests as well
* Continuous evaluation on multiple different angles: measuring the impact of
  the service on sexual health
* Assumptions broken: people don't actually want their results to be
  super-discreet: they just want to know as quickly and clearly as possible
* Prior to alpha - 3 month discovery stage: very intensive working with users


## [Will Rowan](https://twitter.com/thecustomer) - Product Manager at the Ministry of Justice (interview)
* Managers are more important if they have more responsibilities - therefore
  they're predjudiced towards bigger projects and bigger products
* PRINCE2 documentation is large regardless of project size - again
  predjudicing larger projects
* Project example: Multi-car insurance. users not considered, data produced not
  considered, organisational capability to run the service, not considered
  (!!)
* On the importance of competitors: "If you're trying to motivate a project team you need an enemy"
* "The more that you can get product in front of people the better" - internal
  and external. The importance of show and tell. "we can show you what's
  possible" - unlock stakeholder committment: low cost, low-risk "If they can
  quantify the risk they'll sign it off"
* Stakeholders probably don't believe that you can deliver that much in that
  little time. Chip away at this by sharing what you're doing and how you're
  doing it.
* Drew the whole of a product on one A2 flipchart - helped to convince
  stakeholders it's feasible
* Attraction of Waterfall is that it lets you sign off the whole budget. In agile
  you're iterating the budget as well as iterating the project
* Getting everyone speaking the same language is important - e.g. training
  stakeholders to not refer to things as 'portals' when they're nothing of the
  sort
  * If you discover that you've got it wrong, or there's a better term, it
    _is_ worth changing it all the way down the stack
* When running agile: Make the work visible - extremely important: whole
  backlog on the wall. Makes it much easier to have a common understanding.
  Always carry whiteboard markers and post-its when in a non-agile environment
  (you won't find them there).

## [Richard Stobart](https://twitter.com/richardstobart) – CEO, Unboxed Consulting, Agile Coach of the Year 2014
### Techniques for overcoming the Big 7 digital product blockers
* The trap of 100% utilisation: Can't move quickly. Grind to a halt.
* 70% utilisation is about as fast as you can go: helps avoid context-switching
* Intuitive thing when you see someone not busy is to make them busy, but this
  slows down all their other projects
* If you've got spare time, help unblock _other people_, or do nothing": don't
  add more things to the pipeline
* Always be delivering the highest value thing on your most important project
* Optimise 'batch size' - grouping together features for deploy. Small batches
  = responding to change more quickly. = fixing bugs more easily: if the
  problem is a result of code which was written recently, the devs have that
  conceptual stack in their head.
  * Not achievable if your deployment process is onerous or slow
* People used to think of environments like pets: they all had names. Today
  they're like cattle: set up when you need it, tear down when you're done.
* rightscale + ansible + docker: used to spin up 1000 test servers to test
  releasing a product to all students in China (which was the MVP!!)
  * Cost £10k to run that test but was trivial to set up
* Creating roll-back-able database migrations: dbdeploy, liquibase etc. - have
  both database schema versions live in parallel, delete the old one when the
  new one is stable
* In closed-scope projects, everyone feels like they need to throw in all
  their great feature ideas before the scope window closes - leads to feature
  bloat. Solution: get stakeholders on side, and ensure that the product owner
  controls the backlog
