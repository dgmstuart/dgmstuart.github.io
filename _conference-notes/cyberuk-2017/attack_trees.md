---
layout: post
title: "Attack Trees Workshop"
comments: true
date: 2017-03-16
category: liveblog
tags: [security]
---

## Background

- Started with fault tree analysis (e.g. working out why a braking system
  fails)
- NCSC started using attack trees to work out what the real problems are
- Build up the series of things that an attacker needs to do to achieve a
  particular goal
  - look across the whole business to build up the tree
- Helps you to look at the best places to deploy your controls in a
  cost-effective way
- Great way to do security in Agile


## Attack trees at student loans company:

- Looked around to see what the risk discovery process was. There wasn't
  anything
- Desired qualities
  - critiqueable
  - written down
  - scalable for different projects and different importances of assets
  - repeatable
- Needed to justify spending more effort by saying it'd give a really good
  view of risks
- Needed some way of quantifying risks: which ones do we care about more
- Iterative: need to keep coming back to it
- Agile teams like it
- Mix of people in the room: Business and technical
- Easy to update
- Use mindmapping software to capture them
- Multitasking failures: need different people to take different roles in
  these workshops.
- For really serious assets, break up the attack trees into different
  topics: steal money, steal data etc.
- Don't get hung up on the specific numbers: kill those conversations quickly

## Workshop

- Scenario: Cloud document sharing service
- Classes of attack:
  - steal data
  - break system
  - reputational risk
- Next level:
  - Internal, External
    - User types within these - employees, bystanders (e.g. cleaner or
      visitor)
    - Personal error
      - Then the ways that they can get data: install keylogger, steal
        printout, photograph screen etc.
- For attacker: Cost, Complexity, Consequences, Reward
  - Don't talk about the Likelihood: the above things break down the reasons
    for likelihood
- For victim: Damage
- Replay: can it be fixed once (patch a vuln) or is it repeatable (stealing a
  mobile phone, phishing)
- Record Evidence/Rationale against the numbers you chose
- Get consistency between attack trees by having some of the same people in
  each attack tree
- Also make the numbers qualitative: what does it _mean_ for an attack to have
  a specific Complexity value?
- Don't have a midpoint (people will choose it)
- 6 points is good: too big (10) means you get nitpicking, too small (3) is
  too blunt

