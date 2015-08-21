---
layout: post
title: "A Discussion of The Operational Challenges With Unikernels"
date: 2015-08-21
---


## What are Unikernels

Most of this post assumes a basic understanding of what unikernels are
so I’d recommend reading [Unikernels – the rise of the virtual library
operating system](http://queue.acm.org/detail.cfm?id=2566628) before moving
on.

## Why are Unikernels interesting

As a starting point: complexity. Managing infrastructure, and the
software that runs on it, is too complicated. You can impose
organisational rules to control this complexity (we only deploy on
Debian, we only run JVM applications, the only allowed database is
MySQL) but that limits you in other ways too, and in reality is nearly
always broken somewhere in any non-trivial environment (this appliance
uses Ubuntu, this software is only certified on Windows, PostgreSQL
doesn’t run on the JVM). So you turn to software to manage that
complexity; Puppet or Chef do a great job of allowing configuration
complexity to be managed in code (where you can test it) and Docker
allows for bundles of complexity to be isolated from other bundles of
complexity. But there are still an awful lot of moving parts.

Another reason is the growing realisation that security is important.
Securing systems on the internet is hard. Even though the basics are
broadly understood they are often not implemented, and the people
attempting to compromise systems are smart, well paid and highly
incentivised (basically like you). It’s generally easier to break
something than to build it. Part of this is a numbers game – to run a
reasonable sixed system you might need to run 50 different services, and
install 200 packages on every host. An attacker has to compromise just
one of those to win.

A further reason, if one were needed, is the proliferation of many small
internet connected devices, aka. The Internet of Things. Part of this
relates to the above points about security concerns, but some of it is
simply a matter of managing that many single purpose, low power,
devices. The overhead of a typical general purpose operating system and
application runtime just don’t fit this model.

Enter unikernels. Unikernels actually remove unneeded complexity. You’re
running a hypervisor and the unikernel and that’s it. The unikernel
contains only those libraries that you have specifically required. That
drastically reduces the surface area for attack as well as meaning
you’re running less software, hopefully enough less that your power
needs are reduced too. By specifically requiring individual libraries
you’re also making complexity visible. Rather than using a general
purpose operating system with it’s 100s of packages and millions of
lines of code you are at least choosing what to include.

## Operational challenges

While I think some part of the future looks like unikernels their are
some large operational challenges to overcome before they break out of
very specific niches or research projects. Note that

there are architectural and software development challenges as well, I
just happen to think they’re easier to deal with.

### Development environment

There are a few properties of a development environment that I think are
essential to modern development; development/production parity being one
of the most important. Tools like Vagrant, and a move towards
infrastructure as code, and more recently Docker have made great strides
here in the past several years. The different unikernel implementations
are generally based on lesser known software stacks (Haskell, OCaml,
Erlang, etc) so some of this is familiarity. But what does
development/production partity mean for a unikernel based system? We’re
not just talking about the individual unikernel here either – how do I
deploy unikernels? How do I compose several unikernels together to build
an application? What does a Continuous integration or deployment
pipeline look like? In my view the unikernel movement should focus some
efforts here. Not only will this make it easier for people to get
started, but having strong opinions early will allow the nascent
community to solve the problem together, rather than everyone solving it
just-in-time for themselves.

### Managing the hypervisor

I’d argue today most developers don’t spent much time directly working
with hypervisors. Either you’re running on an in-house VMware, KVM or
Xen install with some (hopefully self-service, automated) provisioning
mechanism in place or you’re using a public cloud like AWS, Azure, etc.
The current generation of unikernel systems mainly target Xen. I think
in the short term at least this means getting to know the hypervisor.
Xen is solid software, but I don’t see a great deal of automation around
it – say well maintained Puppet modules, API clients or a Terraform
provider. In the long term we’ll hopefully have higher level interfaces,
but in the short term efforts here would lower the barrier to entry
considerably.

#### Double down on AWS

Given the above, and given the ubiquity of EC2 (which is based on Xen)
it might be wise to build up first-class tools around using EC2 as a
target environment for unikernel deployments. EC2 supports custom
kernels, but these require a number of convoluted steps that could be
automated away (note that I’m talking about more than just a shell
script here). Also what are the best practices around autoscaling groups
andunikernels? Or VPC networks and unikernels?

### The network

With the explosion in containers and microservices it’s becoming clearer
(if it wasn’t already) how important the network is. By removing the
operating system we remove things like host firewalls and the new breed
of overlay networks. At the same time if we are to tap the dynamic
potential of unikernels we’ll need a similarly dynamic and automatable
network. Maybe this becomes more of an application concern, with
services communicating via other services which act as firewalls and
intelligent proxies, but that still leaves the underlying network to be
managed.

### Debugging

However much testing you do beforehand you’ll still likely end up with
problems in production, and as you scale up you’ll hit issues that you
simply can’t recreate outside the live environment. This is were good
debugging capabilities come in. While general purpose operating systems
might be complex they are well know, and tools like ps, top, free, ping,
telnet, netcat, dtrace, etc. are commonly used by anyone debugging
systems. Note that in many cases you’re debugging a combination of
systems; is the performance issue an application problem, a network
problem, a storage problem or some interesting combination of several
facters?

By removing the general purpose operating system, unikernel based
environments remove most of the current debugging tools at the same
time. Part of this Is good application development hygiene (logs,
metrics and status endpoints for instance), but what about the more
interactive debugging practices? What does debugging a system based on
unikernels look like?

### Orchestration

The word may be overloaded but the need to arrange and manage a number
of components that make up a larger system is a real need. This might be
something like Docker's Compose file or [Brooklyn's
Blueprints](https://brooklyn.incubator.apache.org/learnmore/blueprint-tour.html),
or it could be something more akin to the APIs from Cloud Foundry,
Kubernetes or Mesos. Testing some of these models with unikernel based systems
will be an interesting test of how coupled to containers the existing models are.
The lack of legacy again opens up the potential to come up with a truly
modern alternative here too.

## Conclusion

Unless you’re in an environment where security is your number 1 concern
then the current state of Unikernels probably means choosing to adopt
them now is a little bleeding edge. But I think that will change over
time as the various projects mature and address some of the issues
described above. In the meantime I’d love to see more discussion of some
of the operational challenges. I think talking about the needs of
operators at this early stage should make the resulting ecosystems more
robust whsen it comes to future production deployments.
