---
layout: post
title: "Execute ruby code inline in the editor"
date: 2015-03-27 15:41:47 +0000
comments: true
category: tools
tags: [ruby, sublime, vim]
---

I've often wondered how in Ruby screencasts they magically execute code inline
in the editor, e.g. they type:

{% codeblock lang:ruby %}
[1, 2, 3].reverse # =>
{% endcodeblock %}

...and it magically becomes
{% codeblock lang:ruby %}
[1, 2, 3].reverse # => [3, 2, 1]
{% endcodeblock %}

You can see how effective this is in action in
[one of the sample episodes for Ruby Tapas](http://www.rubytapas.com/episodes/11-Method-and-Message)
by the wise and excellent [Avdi Grimm](http://about.avdi.org/).

Today I learned how. Apparently it's a Textmate feature called “Execute and
Update '# =>' Markers” which has been ported over to other editors:

* [Plugin for Sublime Text2](https://github.com/mmims/sublime-text-2-ruby-markers)
* [Plugin for Vim](https://github.com/t9md/vim-ruby-xmpfilter)

_A quick google hasn't revealed how to enable this in Emacs, but it must be
possible because I believe that's what Avdi uses_

This also explains why the snippet for `#` expands to `# =>`. I'd always
assumed that this was intended to make multiline comments more readable. That
sounds a bit silly now that I know :)
