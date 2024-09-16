---
layout: post
title: Inject Roles, Not Objects
date: 2024-09-16 14:51 +0200
comments: true
categories: [ruby, oo-design]
tags: [ruby, rspec, testing, dependency-injection]
---

This post focusses on a specific aspect of Dependency Injection as applied to
Ruby, namely: how should we think about the things that we're injecting?

_Originally posted on 2019-02-04 on the [Varvet](https://varvet.com/)
blog_

Consider the following Ruby code:

```ruby
# app/services/widget_assembler.rb

class WidgetAssembler
  def assemble(options)
    gadget = GadgetAssembler.new.assemble(options)
    Widget.new(gadget)
  end
end
```

It’s an over-simplified example, but contains enough to discuss some ideas about coupling and flexibility.

## Coupling

The amount of information that an object knows about the other objects it interacts with (its ‘collaborators’) is a form of of
[Coupling](https://www.martinfowler.com/ieeeSoftware/coupling.pdf),
and coupling is something we want to minimise, since it makes our code harder to change, and harder to test.

Let’s list out the information that our `WidgetAssembler` knows about this `gadget_assembler` collaborator object:

1. that we’re using a specific object called `GadgetAssembler`
2. it is a regular Class (rather than a module or singleton), since it accepts a `.new` message
3. instances can be built without passing any arguments
4. instances accept an `assemble` method
5. the `assemble` method takes a single argument

## Dependency Injection

A few years ago I was working on a team where all of us had only just started to discover Object Oriented design and were trying to apply it to our Rails project.
In particular we were using
[Dependency Injection](https://en.wikipedia.org/wiki/Dependency_injection)
all over the place and I started seeing code like this:

```ruby
# app/services/widget_assembler.rb

class WidgetAssembler
  def initialize(gadget_assembler = GadgetAssembler)
    @gadget_assembler = gadget_assembler
  end

  def assemble(options)
    gadget = @gadget_assembler.new.assemble(options)
    Widget.new(gadget)
  end
end
```

This is a small improvement: we can cross off point 1, since we’ve decoupled this code from knowing the name of the collaborator by injecting the class object.

_Note: we’re still passing the class object as a default value in the initializer,
but this is just for convenience and it shouldn’t be considered part of the signature of this object.
More on this at the end of the post_.

However `WidgetAssembler` still knows quite a bit about its collaborator,
and there’s still a problem regarding testing.
Our test setup might look something like this
(using [RSpec](http://rspec.info/)):

```ruby
# spec/services/widget_assembler_spec.rb

gadget = instance_double('Gadget')
gadget_assembler = instance_double('GadgetAssembler', assemble: gadget)
gadget_assembler_klass = class_double('GadgetAssembler', new: gadget_assembler)
widget_assembler = WidgetAssembler.new(gadget_assembler_klass)
```

We need to create three doubles here, and we need to stub the `new` method, which feels awkward: the method we actually define is called `initialize`, so it’s not obvious that we entirely own the method that we’re stubbing.

In this implementation we can also see that `WidgetAssembler` has a potentially large surface area of methods that it might expect to call:
not only the methods of the object returned by calling `.new`, but the class methods of the `gadget_assembler` object itself.

## We can do better

What happens if we inject an already-built instance of `GadgetAssembler` instead?

```ruby
# app/services/widget_assembler.rb

class WidgetAssembler
  def initialize(gadget_assembler = GadgetAssembler.new)
    @gadget_assembler = gadget_assembler
  end

  def assemble(options)
    gadget = @gadget_assembler.assemble(options)
    Widget.new(gadget)
  end
end
```

We’re now left only needing to be concerned with two items of information about our collaborator:

- the name of the method we can call on it
- the arguments to that method

Our test setup also becomes simpler, which is usually a sign that we’re moving
in the right direction:

```ruby
# spec/services/widget_assembler_spec.rb

gadget = instance_double('Gadget')
gadget_assembler = instance_double('GadgetAssembler', assemble: gadget)
widget_assembler = WidgetAssembler.new(gadget_assembler)
```

## Injecting roles

Another way to think about this iteration of the code is to consider what we could pass in as the collaborator:

- An instance with `assemble` as an instance method
- A class with `assemble` as a class method
- A module with `assemble` as a module method

Our `WidgetAssembler` really doesn’t care what kind of object the collaborator
is: as long as it plays the _role_ of a `gadget_assembler`, everything works
and no changes are required to the application code.

This is an important point which I feel is often overlooked:
when we start caring about or relying on a particular type of object being injected, we have in a way reintroduced some of the coupling which our dependency injection removed.

This is perhaps in part a side effect of two aspects of working with Ruby which make it easy to fall into thinking of dependency injection in terms of injecting specific objects rather than more abstract roles:

- Firstly, in Ruby we have this nice convenience of being able to initialize an object and pass it as a default value in our initializer. That means we’re writing a specific class name in our application code, even though we could inject any object which fulfils the same role.
- Secondly, in RSpec, in order to make sure that we’re not writing ‘imaginary tests’ where our test doubles don’t actually behave like the real objects that they’re standing in for, we use
  [verifying doubles](https://relishapp.com/rspec/rspec-mocks/v/3-8/docs/verifying-doubles),
  where we again write the name of a specific class, even though the intent of our double is to model a specific _behaviour_ rather than a specific _object_.

## In conclusion...

An often-quoted directive of testing is to "Mock roles, not types", or "Mock roles, not objects".
I think it’s also true that we can think more clearly about dependency injection if we remember to "inject roles, not objects".

## Footnote: on Demeter

When I started writing this post I thought it was going to focus on how
`@gadget_assembler.new.assemble(options)` was a violation of the
[Law Of Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter),
but oddly it seems not to be, despite the fact that there are "two dots".
The Law Of Demeter says that it’s fine for a method to call other methods on
objects which are created inside the method itself.

Perhaps the reason is that in Ruby, classes themselves are objects, and
creating an instance is achieved by calling a method on that object, which is
not the case in many languages. I wasn’t able to get a clear understanding of
how these things fit together, but if you have any insights, please let me
know in the comments.
