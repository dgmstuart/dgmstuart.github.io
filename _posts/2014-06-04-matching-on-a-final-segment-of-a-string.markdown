---
layout: post
title: "Matching on a final segment of a string"
date: 2014-06-04 19:55:14 +0100
comments: true
category: ruby
tags: [ruby]
---

when matching on the final segment of a string, e.g for a file extension, you could use a regular expression:

```ruby
> "foo.bar" =~ /.*\.bar$/
# => 0
> "foo.bar.baz" =~ /.*\.bar$/
#=> nil
```

but of course in ruby there's a much more expressive way
```ruby
> "foo.bar".end_with?(".bar")
#=> true
> "foo.bar.baz".end_with?(".bar")
#=> false
```

you can even pass multiple suffixes as arguments:

```ruby
> "foo.qux".end_with?("bar", "baz", "qux")
#=> true
```

oh Ruby how I love you so.

_[api documentation for end_with?](http://www.ruby-doc.org/core-2.1.1/String.html#method-i-end_with-3F)_
