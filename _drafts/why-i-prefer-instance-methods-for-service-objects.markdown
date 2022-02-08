---
layout: post
title: "Why I Prefer Passing Data to Instance Methods Service Objects"
comments: true
category: [ruby, oo]
tags: [ruby, oo]
---

In this post I want to show an example of an (anti?)pattern which I see in how
service objects are written, describe some issues I have with it, and suggest
a better approach.

## A common pattern

I tend to see service objects which look something like this:

```ruby
class UserNotifier
  def initialize(user, comment_id)
    @user = user
    @comment_id = comment_id
  end

  def notify
    SmsClient.send(@user.mobile_number, message)
  end

  private

  def message
    Comment.find(@comment_id)
    "There's a new comment on your blog post: #{comment.body.truncate(20)}"
  end
end
```

...and so get called like this:

```ruby
UserNotifier.new(user, comment_id).notify
```

Notice that here the user and the comment id are part of the state of the
`UserNotifier` class, so an instance of the class knows how notify a specific
user about a specific comment.

I think there are a number of issues with this:

### Hard to mock....

Let's consider the scenario in which we're using `UserNotifier` in another
class: say we have a `CommentCreator` service that handles everything
that needs to happen when a comment gets added, and one of those things is to
notify the user using our `UserNotifier`.

One thing we will want to assert is in the test of that class is that "the
user gets notified about the comment". This is an
[outgoing command message](https://youtu.be/URSWYvyc42M?t=1401) so we need an
expectation that it gets sent.

How can we do that with this design? (Assuming we're using [RSpec](https://rspec.info/))

We could say:

```ruby
expect(UserNotifier).to have_received(:new).with(user, 123)
```

...but this doesn't actually assert the property we care about: it just
asserts that an instance gets built, not that the `notify` method gets called
on that instance.

The difficulty here is that we need to do two operations in sequence:
Initialise an object with some specific data, then call a method on this
object. That sequence is hard to mock.

We can do it like this...:

```ruby
fake_notifier = instance_double(UserNotifier, notify: false)
allow(UserNotifier).to receive(:new).and_return(fake_notifier)
#...
expect(UserNotifier).to have_received(:new).with(user, 123)
expect(fake_notifier).to have_received(:notify)
```
...but I hope we can all agree that this is pretty nasty: unclear and
difficult to follow

Instead, more often than not tests of objects like this end up asserting
against an object further down the call stack which is easier to make
assertions about. At that point they are no longer unit tests and much is lost
in the process.

### ...and hard to stub

Consider now that we want to test a behaviour of `CommentCreator` which is
dependent on a behaviour of `UserNotifier`: "when a user notification fails,
write a message to the log" (this is maybe not a great example, but work with
me here).

The natural fit for stubbing this is `allow_any_instance_of`, which is a
problem because it is
[widely discouraged, for some pretty good reasons](https://relishapp.com/rspec/rspec-mocks/docs/working-with-legacy-code/any-instance).

It's essentially intended to be used for testing legacy code

Instead we can do something like this:

```ruby
fake_notifier = instance_double(UserNotifier, notify: false)
allow(UserNotifier).to receive(:new).with(user, 123).and_return(fake_notifier)
```

A little less nasty than the mock example, but still something worth avoiding
if we can.

### Injecting dependencies is awkward...

Let's consider how we'd go about applying dependency injection to pass in that
`SmsClient` collaborator in the initializer:

```ruby
class UserNotifier
  def initialize(user, comment_id, sms_client = SmsClient)
    @user = user
    @comment_id = comment_id
    @sms_client = sms_client
  end

  def notify
    sms_client.send(@user.mobile_number, message)
  end

  # ...
end
```

This works, but it feels awkward to me: the things in the list of
initialisation parameters aren't all the same kind of thing:

- `user` and `comment_id` are _data_: they're values which represent nouns in
  the domain
- `SmsClient` is a _collaborator_ - it's another object which we can send
  messages to in order to achieve the behaviour we want.

This feels like a code smell for me because they will _change for different
reasons_:

- The _data_ is expected to be different for each request: let's imagine in this
example that a user only gets notified once for each comment.
- In contrast, the _collaborator_ that we're passing in is going to be the same,
until (for example) something changes in the _system_, like switching to a
different sms provider.

One phrasing of the
[Single Responsibility Principle](https://blog.cleancoder.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html)
(which is the S in
[SOLID](https://simple.wikipedia.org/wiki/SOLID_(object-oriented_design)))
is:

> Gather together the things that change for the same reasons.
> Separate those things that change for different reasons.

This seems to be intended to refer to how code is grouped together into
different objects, but it doesn't feel like too much of a stretch to me to
apply it to initialization arguments.

### ...and injecting this class is even worse
...
...

## Inject collaborators, pass data as arguments

So why do we care if something is hard to mock and stub?

Personally I'm a follower of Test Driven Development, and one of the things
that this approach says is that when your code is hard to test, this is a
signal that the application code maybe has some undesirable properties, and
that if we make some changes to our code which make it easier to test, we
make our code "better" (clearer, less complex, easier to change) in the process.

With that in mind, here's how I would prefer to write this class:

```ruby
class UserNotifier
  def initialize(sms_client: SmsClient)
    @sms_client = sms_client
  end

  def notify(user, comment_id)
    @sms_client.send(user.mobile_number, message(comment_id))
  end

  private

  def message(comment_id)
    Comment.find(@comment_id)
    "There's a new comment on your blog post: #{comment.body.truncate(20)}"
  end
end
```

This would then be called like this:
```ruby
UserNotifier.new.notify(user, comment_id)
```

Note that in this implementation, an instance of the class can be used to
notify _any_ user about any comment.

This comes with some compromises for sure: notice that we now need to pass
`comment_id` as an argument to `message`. I assume that this is the primary
motivation for passing in data to the initialiser, but I think the tradeoff of
passing it to the instance method is very
much worth it for the following reasons:

### Easy to mock...

With this style, passing in a mock object feels pretty clean and descriptive
to me:

```ruby
user_notifier = instance_double(UserNotifier)
comment_creator = CommentCreator.new(user_notifier: user_notifier)

#...

expect(UserNotifier).to have_received(:notify).with(user, 123)
```

### ...and easier to stub

Now our stub is just a one-liner, since we don't care how it's initialised:

```ruby
fake_notifier = instance_double(UserNotifier, notify: false)
```

### Easy to inject dependencies (and config)

Passing the collaborators in the initialiser signals that "these are the
things which define how this class will behave". I didn't include any
configuration in the simple example code, but the initialiser also feels like
a good place to inject things like urls, tokens, timeout lengths, or any other
config which might influence the behaviour of the object.

### ...and easily injected

Since the useful behaviour is in an _instance_, we can pass an instance as the
collaborator:

```ruby
class CommentCreator
  def initialize(user_notifier = UserNotifier.new)
    @user_notifier = user_notifier
  end

  def create!(...)
    # ...
    @user_notifier.notify(user, comment_id)
  end
end
```

### Bonus: re-usable

I'm in general skeptical about the idea of designing code for reusability as a
primary goal (that's a topic for a different blog post), but in this case I
think it's worth considering how the two implemetations stack up:

Consider sending out notifications to multiple users (for the same
`comment_id`)

First implementation:
```ruby
users.each do |user|
  UserNotifier.new(user, comment_id).notify
end
```

Second implementation:
```ruby
notifier = UserNotifier.new
users.each do |user|
  notifier.notify(user, comment_id)
end
```

With the second implementation we re-use the same object to send all the
notifications. I'm not sure I can really justify why this is an improvement,
beyond the fact that it feels better to me: unless we're sending a lot of
notifications at the same time, the performance benefits of this reduced
object allocation are probably minimal.

## Footnote: embrace the new

In some code I've come across in the past, there seems to be a real discomfort
around this `new` which doesn't have any arguments:

```ruby
UserNotifier.new.notify(user, comment_id)
```

This can happen in classes like this where all the initialisation parameters
have default values, so there's no need to pass them when calling the service
in this context.

I've seen things like this:

```ruby
class UserNotifier
  class << self
    def notify(user, comment_id)
      new(sms_client: SmsClient).notify(user, comment_id)
    end
  end

  # ...
end

...in order to be able to call it like this:
```ruby
  UserNotifier.notify(user, comment_id)
```

This feels wholly unnecessary to me - it almost feels like it's adding more
code purely to make the method calls more aesthetically pleasing. It seems
like it's reducing the number of function calls being made, but I feel like
it's actually _adding_ an additional way to call the same method.

What's happening here is kind of like a tiny
[adapter](https://refactoring.guru/design-patterns/adapter) which is so
trivial it shouldn't really exist, but the fact that it's defined in the same
file makes that feel OK.

Personally I'm perfectly fine with that empty `new`. And who knows: maybe it
won't be empty forever. I don't see the value in pretending it isn't there.

