---
layout: post
title: "Operations is more than just Systems Administration"
date: 2015-12-27
---

I think one of the patterns of the last few years has been the
democratization of systems administration, especially for web
applications. Whether that's Heroku or Docker, or Chef or Puppet, more
and more traditional developers are doing work that would have been
_somebody else's problem_ only a few years ago. But running in parallel
to that thread is another less positive trend, that of conflating
operations with _just_ systems administation. The story seems to go that
now we know Ansible (or some other tool) we just need developers to run
the show.

In this post I'm going to try and introduce some of the other
operational disciplines, especially for developers who maybe have come
to operations via the above resurgence in infrastructure tooling over
the past few years.

Note that this post has a slight bias towards more _normal_
organisations. That is to say if you're in a 5 person software startup
you probably don't have operational problems to worry too much about
yet. I'm also not playing down the practice of systems administration,
most experienced sysadmins I know are also quite rounded operations pros
as well.

## Service Management

If you've worked in operations, or in many large organisations you'll have
come across the term Service Management. This tends to be linked to
various service management frameworks; like ITIL or MOF (Microsoft
Operations Framework). The framework will describe, often in great
detail, activities and processes for things like incident response,
configuration management, change management, capacity planning and more.

While I was at [The Government](https://gds.blog.gov.uk/) I wrote what I
think is a reasonable introduction to [Service 
Management](https://www.gov.uk/service-manual/operations/service-management.html)
albeit from a specific point-of-view. This was based on my experience of
trying, and likely sometimes failing, to encourage teams to think about
how the products they we're working on would be run. Each of the topics
touched on in the overview is worthy of it's own stack of books, but I
will repeat the ITIL service list here as (whatever you might think of
the framework or a specific implementation) I'd found it a useful starting
point for conversations - in particular stressing the breadth of
topics under service management.

#### Service Strategy

* IT service management
* Service portfolio management
* Financial management for IT services
* Demand management
* Business relationship management

### Service Design

* Design coordination
* Service Catalogue management
* Service level management
* Availability management
* Capacity Management
* IT service continuity management
* Information security management system
* Supplier management

### Service Transition

* Transition planning and support
* Change management
* Service asset and configuration management
* Release and deployment management
* Service validation and testing
* Change evaluation
* Knowledge management

### Service Operation

* Event management
* Incident management
* Request fulfillment
* Problem management
* Identity management
* Continual Service Improvement

For each of the above points, whether you are using ITIL or not, it's
useful to have a conversation. Some of these areas do provide ample
opportunity for automation and for using tooling to minimise the effort
required. But much of this is about designing _how_ you are going to
operate a service throughout it's lifetime.

## Operations user stories

One of the other things I published while at The Government was a set of
[user stories for a web operations 
team](https://www.gov.uk/service-manual/operations/web-operations-stories.html).
These grew out of work on launching GOV.UK and have had input from
various past colleagues. In hindsight I'd probably do somethings
here differently, the stories assume a certain context which isn't explicitly
spelled out for instance. But they have a couple of things going for them in that
they demonstrate how traditional operations activities can be planned out as part
of a more developer-friendly planning approach, and also they are public and
have been tested by more than a single team.

## Not everything is a programming problem

The main point I think is that not everything can be turned into a
programming problem to solve. Automation has it's place, and many manual
processes and practices can benefit from automation. But the wide range
of activities involved in running a non-trivial and often non-ideal
system in production tend to mean making trade-offs and prioritization
decisions frequently. This is where softer skills like arguing for
funding or additional head count, or building a business case for
further work, come into play. Operations management is much more than
systems administration.


## Further reading

This is little more than a plea for people to think more about
operations, separate to the more technical aspects of systems
administration. If you're interested in learning more however I would
recommend some good reading material:

* [Visible Ops
Handbook](http://www.itpi.org/the-visible-ops-handbook-review.html) -
  still an excellent and pragmatic introduction to many of the topics
  noted above.
* [Designig Delivery](http://shop.oreilly.com/product/0636920033080.do) -
  a bang up-to-date tome covering a range of service design topics.
* [Basic Service
  Management](http://www.basicsm.com/bsm-basic-service-management-book) -
  a 50 page starter book covering the fundamentals of service
  management as generally discussed in more detail elsewhere. A great
  starting point.
