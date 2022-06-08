---
layout: post
title: Programming is a communications discipline, not an engineering one
comments: true
categories: []
tags: []
---

Or maybe it's a bit of both. And maybe it's most true when building
user-facing software, but that's what I've spent most of my career involved in
building, so that's what I'm going to talk about.

I'm going to argue that communication skills and empathy are much more
valuable technical aptitude when building a team.

## NOT ENGINEERS

The term "software engineer" has never sat right with me. Engineering is a
discipline which relies on a rigorous education and a knowledge of and respect
for the foundational principles on which the discipline is built

On the other hand many of the best programmers I've met are largely
self-taught and/or learned on the job. I count myself in that category too: my
degree was in Mathematics and Computer Science (which at my university is a
BA, not a BSc, for odd historical reasons).

In addition, a lot of programmers I've come across are ignorant or dismissive
of the principles and learnings discovered by those who have come before us.
This is often not their fault, and it's certainly possible to have a
successful career, while struggling with same problems that generations before
us struggled with and fixed.

But this is a topic that has been much discussed elsewhere

FIND LINKS

and I want to talk about communication.

Let me tell you a couple of stories.

## SOMETHING - TESTER

One of my first technical roles was as a manual software tester. At the company
I was working at, the programmers didn't write tests for their code - instead
they threw their code over the fence to us and we would follow test scripts:
manually clicking through the different screens of the application to check
that nothing was broken.

I remember one time discussing a bug report from an end user with one of the
programmers. The details are hazy, but it was something like, the user had a
problem with "the close button on the Widget checkout form".

Now, there was no button marked "close" and there was no form called
"checkout", but it was pretty clear from context that what they meant was "the
submit button on the Create Widget form".

But the programmer in question had a really hard time getting their head around
this: "if they meant the submit button, why didn't they _say_ the submit
button?".

## SOMETHING - BA

In another role I had at the same company (I worked there for far too long), I
was a Business Analyst (BA).

BAs had a number of roles, some of which might be in principle useful, like
doing the up-front ground work talking to a lot of different stakeholders to
understand what a new system needed to do and the environment it had to fit
into.

But the main role we had was as translators between the business and the team
of programmers.

The philosophy seemed to be that programmers and business people couldn't
possibly be expected to talk to one another: how could they, when they speak
completely different languages? What was therefore needed were people (us) who
could understand both groups and translate between them.

### SOMETHING requirements lists

This was in the bad old days of
[Waterfall]()
software development, so this translation came in the form of my an enormous
list of requirements, which myself and the other BAs would produce based on
talking to the business.

The problems with this approach were made worse by the fact that the handoff
was always in the form of a document that we handed over to the programmers,
often before moving on to a different project.

The result was a game of
[Telephone]():
It's just not possible to succinctly specify how software is expected to work
in a list of requirements, and even if we BAs had correctly understood the
intention of the business, the chances that the programmers would interpret
what we had written correctly was slim.

In some cases we stuck around on the project and were able to answer questions
and clarifications about what was supposed to be built: advocating for the
stakeholders (if not the end users). Those were the better projects.

## A better way

> "Any fool can write code that a computer can understand. Good programmers write code that humans can understand."
>
> _-_[_Martin Fowler?_](https://en.wikiquote.org/wiki/Martin_Fowler#Refactoring:_Improving_the_Design_of_Existing_Code.2C_1999)



