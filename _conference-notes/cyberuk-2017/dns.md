---
layout: post
title: 'NCSC + Nominet on DNS - "Active Cyber Defence"'
comments: true
date: 2017-03-16
category: liveblog
tags: [security]
---

- DNS service for all levels of UK public sector, which will not resolve
  domains known to be malicious
  - fringe benefit: NCSC knowledge of what malware exists across gov networks
- Nominet
  - sitting on a lot of DNS data
  - Built their own DNS resolver - Turing - wrapping analytics and blocking
    etc. around that
  - Analysing events to identify potential bad behaviour
  - Integrates with third party security feeds
  - Uses machine learning to remove the noise to a manageable scale
  - Establishes a 'normal' signal profile - allows identifying unusual
    behaviour
- Attack: making lots of random subdomain requests to nameservers of a bank -
  eventually makes them go down
- Other things which can be detected:
  - Data exfiltration - very easy to spot usage of exfiltration tools (iodine)
  - Locky DNS (?)
  - Spam campaigns are easy to identify from DNS records

