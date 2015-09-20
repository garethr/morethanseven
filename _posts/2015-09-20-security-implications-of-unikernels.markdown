---
layout: post
title: "Some Security Implication of Unikernels"
date: 2015-09-20
---

I was attending the first [GOTO London conference](http://gotocon.com/goto-london-2015/) last week, in particlar the [Rugged](https://www.ruggedsoftware.org/) Track. One of the topics of conversation that came up was unikernels, and their potential for improving the state of software security. Unikernels are pretty new outside research groups, I’m just lucky enough to live and work in Cambridge where some of that research is happening. The security advantages of unikernels are one of the things that attracted me in the first place. I thought it might be interesting to jot a few of those down for other people interested in security and the future of infrastructure.

As with my [last post](http://www.morethanseven.net/2015/08/21/operating-unikernel-challenges/), it’s worth having a basic understand of Unikernels. I’d recommend reading [Unikernels - the rise of the virtual library operating system](http://queue.acm.org/detail.cfm?id=2566628).

## Hypervisor

Every unikernel is provided the isolation guarantees from a hypervisor. Not only are these guarantees reasonably well understood, they tend to make use of hardware features too. It’s interesting to note that recent container runtime work is heading in this direction too, with ptojects like [Clear Containers from Intel](https://clearlinux.org/features/clear-containers), [Bonneville from VMware](https://blogs.vmware.com/cloudnative/introducing-project-bonneville/) and the [new stage1 in rkt](https://coreos.com/blog/rkt-0.8-with-new-vm-support/).

## No User Space

With a typical server OS we have kernel space and user space. Part of the idea here is to ensure the underlying machine doesn’t crash, whatever horrible things people do in user space. But this means _you can do horrible things_. The unikernel model is similar to the Erlang philosophy of _let it crash_. You only have kernel space, you entire application resides in it. Most things out of the ordinary are going to crash the kernel. This makes the sort of exploratory testing useful in exploit development harder.

## Really Immutable Infrastructure

People often talk about immutable infrastructure. I’d wager there is more talk than reality however. When you push, people are often not using read-only file systems and retain the capability to login to machines to make ad-hoc changes. What they mean by immutable is that they only change machines at deploy time. This ignores both the fact they have the technical capability to change them anytime, and that an attacker could change them outside that deployment cycle. With unikernel systems there is often just the compiled kernel, you can’t just change files on disk. The defaults force an immutable way of working.

## Clean Slate TLS

As a typical developer or operator you’ve probably learned more than you wanted to know about the OpenSSL source code. It’s not well understood and not likely to be so anytime soon and has some pretty spectacular bugs like [Heartbleed](http://heartbleed.com/). The [Core Infrastructure Initiative](https://www.coreinfrastructure.org/) is laudable and will improve things but it’s still a problematic codebase. Functional programming is often regarded as an easier way of writing understandable code. Types are a good thing, especially when it comes to security systems. So a pure [OCaml TLS](https://mirage.io/blog/introducing-ocaml-tls) implementation as used by [MirageOS](https://mirage.io/) makes sense on lots of levels. Yes this is quite an undertaking, but the [bitcoin pinata](http://amirchaudhry.com/bitcoin-pinata/) tests show promise.

## Formal Proofs

Knowing whether an application really does exactly what you want it to do (and no more) is a hard problem to solve. Unit tests and other form of automated testing help, but are still reliant on people to both write and design the tests. A formal proof system can provide much stronger guarentees of correctness, it’s an approach used in some cases for missing-critical components of [Amazon’s AWS](http://cacm.acm.org/magazines/2015/4/184701-how-amazon-web-services-uses-formal-methods/fulltext). MirageOS is implemented in OCaml. One of the most popular OCaml programmes is [Coq](https://coq.inria.fr/), which just so happens to be a formal proof management system. I’ve not seen many examples yet of this approach, probably due to the effort involved, but the capability is there for building formally specified unikernels. I’d wager a similar thing is possible with Haskell and [HalVM](https://github.com/GaloisInc/HaLVM). Making that easier to do for typical developers could open up much more secure development practices for certain usecases.

