---
layout: post
title: "Three approaches to data modelling for calendar events"
date: 2014-08-02 00:39:30 +0100
comments: true
category:
tags: [design, "data modelling", ruby, rails]
---
!["A sketch of Swing Out London's data model"](/images/content/soldn_data_model_sketch.jpg)

[Swing Out London](http://swingoutlondon.co.uk) has to deal with a set of constraints shared by any moderately complicated system handling events which have dates associated with them:

* Storing repeating events (in this case, only weekly events)
* Storing one-off events
* Generating a single list showing all events
* Updating events
* Representing one-off cancellations and exceptions

When you start to get into this it quickly becomes very challenging to support this in a flexible, maintainable and performant manner. This post describes a couple of blind alleys I went down, and a solution I've settled on which I'm pretty happy with:

1. [Approach 1: Calculate at runtime](#approach1)
1. [Link to Header](#approach2)
1. [Link to Header](#approach3)

### Repetition

The simplest case of repeating events is those which repeat every week, since every event will always be exactly 7 days apart.

This happens to be the only case which Swing Out London deals with so e.g. monthly repeating events are handled in the same way as one-off or intermittent events (_this is a deliberate choice which I might discuss in a future post_).


TL;DR
-----
In the solution I've settled on, four model objects are responsible for managing event data, each with a single responsibility:

* **Event** - groups together **Event Seed**s which refer to the same event
* **Event Seed** - holds the data which is usually the same from instance to instance, and references to **Venue**, **Organiser** etc.
* **Event Generator** - holds data about dates and repeating behaviour. Generates **Event Instance**s
* **Event Instance** - inherits most of its data from Event Seed, but can override it. Has a date.

Background
----------
* [Swing Out London](https://github.com/dgmstuart/Swing-Out-London) is a cms for managing swing events
* I'm rebuilding it from scratch as [a totally new project](https://github.com/dgmstuart/swingoutlondon2)
* The current live system is Rails 3, the rebuild is in Rails 4
* Both versions use a postgres database



<a name="approach_1"></a>Approach 1: Calculate at runtime
---------------------------------

Do-able with modulo math in the sql

[old data model](https://github.com/dgmstuart/Swing-Out-London/blob/274f64e1d635bcd8d2678eb6a0dfa50516ef64ba/db/schema.rb)


<a name="approach_2"></a>Approach 2: Generate events
---------------------------------

<a name="approach_3"></a>Approach 3: Generate instances
---------------------------------

{% highlight ruby %}
# app/models/event.rb
has_many :event_seeds
has_many :event_generators, through: :event_seeds
has_many :event_instances, through: :event_seeds

# app/models/event_seed.rb
belongs_to :event
has_many :event_generators
has_many :event_instances

# app/models/event_generator.rb
belongs_to :event_seed
has_one :event, through: :event_seed
has_many :event_instances, through: :event_seed

# app/models/event_instance.rb
belongs_to :event_seed
has_one :event, through: :event_seed
{% endhighlight %}


[new one](https://github.com/dgmstuart/swingoutlondon2/blob/daa4397f1e9d772a5b5302cdd369b81201c8ec84/db/schema.rb)

Approach 3a: Instances: create one-offs, generate repeating
