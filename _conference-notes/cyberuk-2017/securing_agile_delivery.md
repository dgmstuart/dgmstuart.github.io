---
layout: post
title: "How to Build Security into An Agile Team"
comments: true
date: 2017-03-16
category: liveblog
tags: [security]
---

- People-centered view as much as process and technology
- Modern technology orgs:
  - Low mean time to recover (MTTR) from cyber attacks
  - Practicing responding to attacks: not just waiting for a real attack. This
    is pretty rare, esp. in government.
  - Each aspect can't be transformed independently

# Transforming security

- Don't try and force existing processes into the new security environment
  straight away: it won't fit
- Accept immaturity: modern practice and technologies are inherently less
  mature than the bad old ways
- Avoid tools designed for waterfall delivery
  - Don't do 'point-in-time' assurance: it's not compatible with continuous
    delivery
  - Avoid change control by decoupling systems to be tolerant of failure in
    other systems (?)
- Manage the boundaries:
  - need the support of high-up people in the organisation
- The right people:
  - Need technical experts right from the start
  - Pioneers (at the beginning) << Wardley model: Pioneers, Town Planners,
    Settlers >> - people who will get shit done right at the beginning
  - Non-conformists: will challenge the status quo.

# Deliver

- Embed security expertise in the team
- It's everyone's problem - all across the functions in a team.
- Use scope reduction to limit risk: cut down exposure by cutting down scope
- do _continuous_ assurance rather than point-in-time assurance
  - smaller pieces but regularly
  - to start with, when you're only exposing your service to a small group of
    users, it's OK to have large areas of unmitigated risk, or even unexplored
    risk
- Capture the standards: define what modern practice means specifically: in
  terms of what Continuous Deliver means for your org, what standards cloud
  platforms must meet (e.g. multiple availability zones)

- Continuous security delivery: Aim is to e.g. be able to patch systems en
  masse because they have the relevant hooks to do so

# Tech as a service

- manage risk associated with user-facing services separately from the
  lower-level infrastructure and common systems (e.g. hosting) - this lets
  you avoid having to repeat the same tasks for each service.
- (Analogy: GaaP)

# Situational awareness

- Security Operations Centre is an antipattern for IT
  - Security theatre: gives the impression of lots of work being done
- Need to know about all technology, including shadow IT. Need some way to
  manage a sustainable technology register
- Need to know the Health of all the tech - is it working as expected?
  - uptime checkers are easy to automate
  - With legacy systems it might need to involve a phonecall 😱
  - Outsourcing complicates things
- Need experts to help tune and fix the technology
- Need to know what accumulated risks are being taken
  - Keep accepting incremental increases of risk: "boiling the frog" - can get
    to places where your risk appetite is below what you have in reality

# Evergreen technology

- need to be able to change rapidly
- Metric: average age of technology
  - seems to be on the increase (at least in MoJ)
  - Potentially the biggest threat to IT security
  - Not sustainable for non-massive organisations
  - Massive vendors can just drop products which are too hard to manage.
    Government can't just stop delivering a service

