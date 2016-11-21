---
layout: post
title: "Unikernels and The End of the General Purpose Operating System"
date: 2016-11-21
---

The [previous
post](http://www.morethanseven.net/2016/11/05/the-end-of-the-general-purpose-operating-system-as-it-happens/)
went into why I think the days of the general purpose operating system
(for servers) are numbered. But one interesting area I didn't comment
on (but did talk about in the talk of the same name) was Unikernels.

## It's all about cost

One of the topics I didn't really touch on in discussing the end of the
generally purpose operating system was cost. Historically,
maintaining a general purpose operating system has been a costly
endeavour, something only the largest companies or communities could
sustain by themselves. Think Red Hat, Oracle, Microsoft, Sun, IBM,
Debian, etc. The result of that is the assumption when building software
that you should target one or more of a small number of operating systems.
In doing so you're ceding some ground, and likely some revenue, to another
vendor. You're also stuck with any underlying limitations of that OS as
well as its release cadence. And invariably you're also stuck with the
multiplying support cost of supporting your software on multiple versions of
that OS over time.

I would posit that up until relatively recently the cost of that support
burden was hugely outweighted by the cost of maintaining an actual operating
system. But that's now changing, as I outlined in the previous post. Now a
small or medium sized software company (be it CoreOS, Rancher, Docker,
Pivotal, etc.) can build and maintain it's own operating system as well.
This is very much about the rising level of abstraction - all of the
above leverage the huge efforts that go into the Linux kernel and into
other projects like systemd (CoreOS) or Alpine (Docker's Moby) for
instance.

## Enter Unikernels

But where do Unikernels fit into this narrative? I'd argue that they
represent the fulfilment of this democratization. If building and
maintaining a traditional OS is only possible for the largest of
companies, and building and maintaining a more special-purpose OS (say
for running containers, or a storage device) is cost-effective for medium
sized softare companies, then Unikernels will allow anyone to build their
own single-purpose operating systems.

There are other technical reasons for (and against) Unikernels as an
approach but most focus on the technical. I think the economic side is
worth some consideration too. And not just the typical development and
support costs, but the ability to own the end-to-end unit of software
has lots of benefits, and Unikernels may make those benefits available
to everyone, including small organisations and individuals.
