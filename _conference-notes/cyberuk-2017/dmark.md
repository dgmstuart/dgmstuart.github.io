---
layout: post
title: "Demos on NCSC tooling"
comments: true
date: 2017-03-15
category: liveblog
tags: [security]
---

## Domain service

- Hard to work out what domains a dept owns and what they're responsible for
- Used whois to "build the DNS tree for government"
  - domains and subdomains
- domain prototype service on ncsc.gov.uk (not public yet)
  - see all domains owned by a particular organisation and see whois records
- helps to find e.g. which gov depts were on cloudflare (and thus would be
  victims of the cloudbleed event)

## WebCheck

- point it at a domain and it'll score based on potential vulns

## DMARC

- Terraform: open source - config files for infrastructure (?).
  https://www.terraform.io/
  - Configuration as code. Source-controlled
- Need to protect even domains which don't send email: other people might be
  trying to send email from that domain
- mailcheck: open source https://github.com/ukncsc/mail-check
  - Third party equivalents exist as Saas
- NCSC want people to send their DMARC reports to them: feed awareness of
  what is happening in gov
  - Starting to do analysis to see if there are dodgy links in these emails
    and flag them for takedown
- DKIM - tell mail provider to sign outgoing email

## Questions

- Currently public sector, may extend to academia etc

