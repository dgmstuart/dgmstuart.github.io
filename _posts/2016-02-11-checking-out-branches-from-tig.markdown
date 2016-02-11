---
layout: post
title: "Checking Out Branches From Tig"
date: 2016-02-11T12:48:36+00:00
category: [ git ]
tags: [ tig, git ]
---
I use [tig](https://github.com/jonas/tig) for my git tree browser. My normal
usage is to look at the the commit tree (normally viewing all branches with
`tig --all`) but it's a powerful tool with a lot of features, some of which
I'm starting to integrate into my daily work.

Today I learned how check out a branch directly from tig.

The first
step is to switch into 'refs' mode with `r` - this shows a view of all the
local branches, remote branches and tags:

{% img /images/content/tig_refs_view.png %}
_tig's 'refs' view_

From any of these you can check out that branch or tag by putting the cursor
on that line and pressing `C`.

On my team we use long branch names which
reference a story ID, so checking out with tig is much quicker than typing out
eg:

    git checkout -b branchname origin/branchname

It's also possible to check out individual commits (without creating a branch)
from the normal tig view by putting the cursor over that commit and pressing
`O`.
