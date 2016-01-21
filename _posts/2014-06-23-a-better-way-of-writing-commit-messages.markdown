---
layout: post
title: "A better way of writing commit messages"
date: 2014-06-23 23:52:20 +0100
comments: true
categories: [Development]
---
I've always tended to write commit messages by answering the question "What did I do?", but I learned a different approach recently which tends to produce much more expressive commits, which are often more terse as well. Compare the following:

 "What did I do?" | "What do things look like now?"
------------------|---------------------------------
Fixed a bug whereby active links were displaying in the wrong colour | Active links now display as blue
Changed the styling of the popup for clarity | The popup now more clearly displays the review description
Fixed styling bug in previous commit | The styling added in the previous commit now only gets applied to the modal
Added a list of reviewed versions with icons to the security column | The security column now displays a list of reviewed versions

Ok, so not all of these are fantastic commit messages (they're all real from recent work I've done). This approach isn't a magic formula but it helps me to focus on what's most relevant, and I feel like the messages on the right are more useful.

An important insight is that although at the time the distinction between implementing a feature/fixing a bug/adding a tweak seems important, when scanning through a list of commits, it's probably the actual behaviour which which is going to be most useful _(caveat: if you have a story ID or ticket reference then it's probably worth including)_.

_Props for the tip go to [zeeraw](https://twitter.com/zeeraw) - my colleague at [dxw](http://www.dxw.com/)_