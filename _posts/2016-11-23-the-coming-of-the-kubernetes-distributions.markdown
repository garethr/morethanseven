---
layout: post
title: "The coming of the Kubernetes distributions"
date: 2016-11-23
---

Very few people today start using Linux by downloading the linux kernel
and starting from scratch. Most people start with a Linux distribution;
for instance Debian, Ubuntu or CentOS. These distributions provide some
opinions, some central infrastructure, a brand, strong versioning for
the entire ecosystem and a bunch of other things. I posit that we'll see
the same pattern emerge with Kubernetes.

## What even is Kubernetes?

I've seen Kubernetes described as all of the following:

* An operating system for your datacenter
* The distributed systems toolkit
* The Linux kernel for distributed systems

I think all of these descriptions point to the developers intent that
Kubernetes is something to build upon, rather than a simple out-of-the-box
experience. It's predominantly about building agreement on the
primitives/APIs of distributed systems.

## A name for a thing

I've not seen much discussion of this in general yet, I think because
it's early days and many of the people looking at Kubernetes today are
either developers or early adopter types. These people have been
"downloading the kernel and starting from scratch", even until recently
most likely running from source downloaded directly from GitHub. If the
Kubernetes ecosystem is to grow then that's not how more mainstream IT
will adopt Kubernetes.

The reason for discussing this now is that I think a name is useful.
That way we can talk about Kubernetes (singular, the software) separate
from distrubutions of Kubernetes (many of them, from different vendors
and communities). I'd be happy to see a different name, but I think
distribution probably fits best.

## Any evidence?

Absolutely. A range of software vendors are providing what I'm calling
Kubernetes distributions. Here is a sample, I'm sure there are and will
be more. I'm also sure over time some will disappear or maintain only a
niche audience.

* OpenShift from Red Hat
* Tectonic from CoreOS
* Kismatic from Apprenda
* Rancher
* Canonical Distribution of Kubernetes
* GKE from Google
* Azure Container Service from Microsoft
* Photon Platform from VMware
* Navops from Univa

Note that Canonical are already using the term _distribution_ in the
name. I've seen it used in passing in CoreOS, OpenShift and Apprenda
press materials too.

## What can we expect from Kubernetes distributions?

Running with the analogy that Kubernetes is "an operating system for
your datacenter" and that we'll have a range of competing Kubernetes
distributions, what else can we expect over the next few years?

### Package repositories (aka. app stores)

One of the things provided by the traditional Linux distributions has
been a central package repository. Most of the packages you're
installing from `apt` or `yum` are coming from that currated set of
available packages. Not to mention community efforts like EPEL. We
already have two package concepts within the Kubernetes ecosystem -
container images (often from Docker Hub today, or from internal
repositories) and Charts, part of the Helm package management tool
(now a CNCF project).

In the short term expect the shared public Charts repository and Docker
Hub to dominate. But over time different vendors will launch there own
repositories. Partly this will be about building a trusted ecosystem,
partly about limiting permutations for support and testing, and partly
about control. The prize here is to be "the enterprise app store" and
no vendor in this space isn't going to at least try to own that as part
of their platform.

### Kubernetes standards and compliance

In an environment with many distributors of core software, it's
common for people to emphasise portability. As vendors extend their
distribution (to provide higher level, but potentially proprietary
features) this can become muddier. Some level of certification is
often the answer. See CloudFoundry or OpenStack for recent examples.
Kubernetes is already part of the CNCF, part of the Linux Foundation.
I'd expect to see the works standards and certification eventually
float around, but my guess is not in the short term.

### A fight over who is the most open

Much of the container conversation recently has centered around a
_weaponisation_ of _open_. I think as the different distributions try and
take the community with them at the same time as trying to scale sales this
will continue. This will be an irritation and is probably best avoided.

### Pressure for AWS to offer Kubernetes as a service

I would presume AWS has a very good idea of how many people are actually
using Kubernetes on it's platform. I think as that grows, and as other
vendors efforts mature, they will come under pressure to offer the
Kubernetes API as a service. I'm still split on whether that will
actually happen but that's a longer blog post about economics.

### Differentiating features

Ultimately vendors will try and differentiate themselves in this new
market. To begin with the majority of business will be targetting the
container-curious and mainly talking up the benefits of containers and
Kubernetes. But some potentialy customers are going to insist on
comparing Kubernetes distributions and winning there is going to be about
clear differentiation. Do you want to be the budget offering or the
provider with the unique selling point?

## Interesting questions

An observation at the moment is that all the current Kubernetes
distributions I'm aware of are vendor-owned. Whether Open Source or not,
they are driven by a single vendor (CoreOS, Red Hat, Apprenda, etc.)
It's interesting to see whether, in the current climate, we see a
genuinely free and open source Kubernetes distribution emerge, similar
to the role Debian plays in the Linux distribution world.
