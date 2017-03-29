---
layout: post
title: "Agile Revisited - Dan North"
comments: true
date: 2017-03-15
category: liveblog
tags: [security]
---

## 90's

- Large projects
- Delivery every 18 months = considered good
- Functional silos: BAs => Systems analyst => Programmers => Testers => Audit
  - Systems analysts (now solutions architects) - lots of diagrams
- Tech was slow and fragmented
  - couple of lines of code: 30m to compile, 2h to link
  - pushes people to write more code at the same time
- Process modelled on Civil engineering
  - Computers came to industry via defence
  - << Asked who has a parent in IT - 2 people in the room >>
  - "We are literally making it up"
  - When we don't know what to do, we look for analogies and patterns from
    what exist already
  - Cost of errors grows more and more the further down the line you go
    - therefore do assurance through formal hand-offs in order to catch
      problems earlier
  - Plans are intolerant of slippage: a delay in one place pushes everything
    downstream
- Made _assumptions_ that the errors rise exponentially and that assurance
  needs to happen through formal handoffs
  - These actually reinforce the other aspects which are like Civil
    Engineering

## Agile

- Bob Martin & Co
  - Come up with their own little methodologies: scrum, xp etc.
  - Went to a ski resort and produced the Agile Manifesto
- The manifesto is about what we value: the things on the right are not
  worthless, just less valuable than the things on the left
- "Complex adaptive system" - feedback feeds into what you build next
- "Two people conversing is the highest fidelity communication you can have"
- << He always tries to escalate to one step higher in fidelity: "If you email
  me I'll phone you" >>
- You can get reusability from simplicity - the other way round doesn't work

## Agile going wrong

- One of the principles "won" - throttled all the others
- SCRUM has a bunch of 'stuff' in it
  - couple of people decided that they could make a lot of money by selling
    certification in SCRUM
- In silo-ed orgs "Everyone outside my silo is an idiot and I'm the one
  holding the company together"
  - Everyone tries to lock everything down so that they can pass it down to
    the next layer
- Original idea: SCRUM masters bring together cross functional teams who don't
  trust each other or speak the same language. Product owner brings people
  together under the banner of building a project
  - Reality: SCRUM masters recruited from Project Managers - totally different
    personality type
- SCRUM doesn't care about lots of the principles

## Agile Now - 2010s

- "Agile Methodology" is a misnomer
- Smaller projects - monthly drops not unusual
- Feature teams going off and using lots of different technologies: an
  explosion of things to support
- Things which need to happen _across_ these teams don't happen because
  nobody is responsible
- We're often building Incrementally (block by block), but not Iteratively
  (sketch, then fill in the lines)
- Upstream: money is batched - this is a problem for agile
- Kanban, SCRUM etc fixes team-level concerns, not the bigger picture
  - governance, steering, assurance
- Success from enaging security people really really early

## Agile Next

- Core tenet of Lean: Mode the people to the work
  - NOT handoffs between silos
  - Remove bottlenecks: if someone is the one expert in a particular system -
    train up 3 people so that they can unblock that
  - Look ahead, and fit the structure of the team to the work that needs to be
    done
- Quote (?) Autonomy - permission and authority to make the changes I need to
  make, Mastery, Purpose
- Build your own lightsaber - build your own process << But my question is:
  what about 'not invented here'? >>
- "Toyota builds people, people build cars"
- Embrace radical diversity
  - go out of your way to bring together people with different backgrounds
    _because you get better work!_
- Measure business impact
- Less is more - like surgery
- Developer "productivity" isn't a thing:
  - a lot of thinking for a small change which is the right change is time
    well spent!
- Shortest path between writing a line of code and someone benefiting
- Monitoring should also be instant and free
- Two week sprints are mini waterfalls
- Requirements emerge as a series of hypotheses
