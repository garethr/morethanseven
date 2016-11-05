---
layout: post
title: "The End of the General Purpose Operating System"
date: 2016-11-05
---

As interesting chat on Twitter today reminded me that not everyone is
probably aware that we're seeing a concerted attempt to dislodge the
general purpose operating system from our servers.

I gave a talk about some of this [nearly two years
ago](https://speakerdeck.com/garethr/the-end-of-the-general-purpose-operating-system)
and I though a blog post looking at what I got right, what I got wrong
and what's actually happening would be of interest to folks. The talk
was written only a few months after I joined Puppet. With a bunch
more time working for a software vendor there are some bits I missed in
my original discussion.

## What do you mean by general purpose and by end?

First up, a bit of clarification. By _general purpose OS_ I'm referring
to what most people use for server workloads today - be it RHEL or variants
like CentOS or Fedora, or Debian and derivatives like Ubuntu. We'll
include Arch, the various BSD and opensolaris flavours and Windows too.
By _end_ I don't literally mean they go away or stop being useful. My
hypothosis is that, slowly to begin with then more quickly, they cease
to be the default we reach for when launching new services.

## The hypervisor of containers

The first part of the talk included a discussion of what I'd referred to
as _the hypervisor of containers_, what today would more likely be
referred to as a CaaS, or containers as a service. I even speculated
that VMWare would have to ship something in this space (See vSphere Integrated
Containers and the work on Photon OS) and that counting out OpenShift
would be premature (OpenShift 3 shipped predominantly as a Kubernetes
distribution). I'll come back to why this is a threat to your beloved
Debian servers shortly.

## The race to PID1

For anyone who has run Docker you'll likely have wrestled with the
question of where does the role of the host process supervisor (probably systemd)
start and the container process supervisor (the Docker engine) end? Do
you have to interact directly with both of them?

Now imagine if *all* of the software on your servers was run in containers.
Why do I need two process supervisors now with 100% overlap? The obvious
answer is you don't, which is why the fight between Docker and systemd
is inevitable. Note that this isn't specific to Docker either. In-scope
for [cri-o](https://github.com/kubernetes-incubator/cri-o) is _Container
process lifecycle management_.

## Containers as the unit of software

Hidden behind my hypothosis, which mainly went unsaid, was
that containers are becoming the unit of software. By which I mean
the software we build or buy will increasingly be distributed as
containers and run as containers. The container will carry with it
enough metadata for the runtime to determine what resources are
required to run it.

The number of simplying assumption that come from this shared contract should not
be underestimated. At least at the host level you're likely to need lots
of near-identical hosts, all simply advertising their capabilities to
the container scheduler.

## Operating system as implementation detail

What we're witnessing in the market is the development of vertically integrated
stacks.

* Docker for Mac/Windows/AWS/Azure ships with it's own operating
  system, an Alpine Linux derivative [nicknamed Moby](http://lucjuggery.com/blog/?p=753), which is not intended for direct management by end users.
* Tectonic from CoreOS is a Kubernetes distribution which runs atop a
  cluster of managed CoreOS hosts. Most of the operating system is
  managed with frequent atomic rolling updates.
* OpenShift Enterprise from RedHat is another Kubernetes derivative,
  this time running atop Atomic host.
* Pivotal CloudFoundry [ships with the IaaS, host OS, kernel, file
system, container OS all tested
together](https://twitter.com/jambay/status/794904634502496257)

In all of these cases the operating system is an implementation detail
of the higher level software. It's not intended to be directly managed,
or at least managed to the same degree as the general purpose OS you're
running today.

This is how the end comes for the majority of your general purpose
operating system running servers. The machines running containers will
be running something more single purpose, and more and more of the
software you're running will be running in containers.

The reason why you'll do this, rather than compose everything yourself, is
compatability. Whether it's kernel versions, file system drivers,
operating system variants or a hundred variations that make your OS
build different from mine. Building and testing software that runs
everywhere is a sisyphean task. Their is also the commercial angle at
play here, and the advantage of being able to support a single validated
product to everyone.

## Implications

There are lots of implications to this move, and it's going to be
interesting to see how it plays out with both early adopters and
enterprise customers alike.

* What does this mean for corporate operating system policies?
* How do standard agent-based monitoring systems work in a world of
  closed vertical stacks?
* Will we see this pattern for other types of service in the AWS Marketplace,
  where instance launched are inaccessible but automatically updating?
* How does such fast moving software work in environments with rigid
  change control processes or audit requirements?
* Many large organisations will end up running more than one of these
  types of system, how best to manage such heterogenous environments?
* Will we see push back from some parties? In particular the open source
  community who may see this mainly serving the needs of vendors?
* Does the end of the general purpose OS lead to greater specialism
  amongst systems administrators?

I'd love to chat about any of this with other folks who have given it
some thought. It's interesting watching grand changes play out across
the industry and picking up on patterns that are likely obvious in
hindsight. And if you like this sort of thing let me know and I'll try
and find time for more speculation.
