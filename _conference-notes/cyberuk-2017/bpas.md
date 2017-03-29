---
layout: post
title: "The response to an attack on BPAS"
comments: true
date: 2017-03-14
category: liveblog
tags: [security]
---

## British Pregnancy Advice Service

- pregnancy, abortion, advice etc.

## Context

- site was due for renewal at the time
- thought that had made clear to dev to not hold sensitive information (but
  this hadn't been communicated to the contractor
- hired someone who didn't have the skills to build the CMS they needed
- Contact form:
  - Supposed to only send an email, not to store the data
  - This built up over 6 years (9000 html pages), but relatively small
- Server monitoring was good - just the site was bad
- Nobody believed it had anything sensitive on it, so the security wasn't
  given significant attention

## Attack

- Hacked by anonymous - anti-abortion defacement
  - SQLi
  - Malware to allow them back in
  - Posted that he was holding sensitive info about BPAS info
- Source control helped to identify the files changed by the attacker
- He seemed to have been using an evaluation copy of the hack software which
  didn't mask his IP

## Cleanup

- Fast injunction on the data (12 hrs)
- Took down server and republished
- Reported to Met Police
  - Arrested within 24h
  - Prosecuted - 32mo inside
- Report to ICO

## Aftermath

- Large increase in hack attempts after the news story
  - incl Low Orbit Ion Cannon
- Investigation took 2 years
  - No visit to BPAS from ICO
- Hard to find records about original sites
- No hard evidence that they had communicated to the contractor that no
  personal data should be stored
- Fine £250,000
- Ignorance is no excuse: ICO said that they should have known
- They assumed that _they_ were not under investigation. Should have gotten
  Lawyers involved earlier
- The negative publicity and misunderstandings were quite damaging to the
  organisation

## Costs

- Actual attack was negligible
- Bad publicity was significant
- Anxiety for clients
- Site down for 3 days
- Follow-on attacks put IT contractor relationship at risk
- The fine was from charitable funds

## Stats

- Most leaks are a result of data sent to the wrong person - actual attacks
  are pretty rare
