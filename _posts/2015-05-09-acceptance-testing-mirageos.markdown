---
layout: post
title: "Automating windows development environments"
date: 2015-05-09
---

I'm pretty interested in [MirageOS](http://openmirage.org/) at the
moment. Partly because I find the idea behind unikernels interesting and
partly because I keep bumping into the nice folks [OCaml
Labs](http://www.cl.cam.ac.uk/projects/ocamllabs/) in Cambridge.

In order to write and build your MirageOS unikernel application you need
an OCaml development environment. Although this is
[documented](http://openmirage.org/wiki/install) I wanted something a
little more repeatable. I also found and reported a few bugs in the
documentation which got me thinking about acceptance testing. I'm not
(yet) an OCaml programmer, but infrastructure automation and testing I
can do.

## Into Puppet

I started out writing a Puppet module to install and manage everything,
which is now available on
[GitHub](https://github.com/garethr/garethr-mirageos) and on the
[Forge](https://forge.puppetlabs.com/garethr/mirageos).

This lets you do something like the following, and have a fully working
MirageOS setup on Ubuntu 12.04 or 14.04.

```puppet
class { 'mirageos':
  user      => 'vagrant',
  opam_root => '/home/vagrant/.opam',
}
```

Given time, inclination or pull requests I'll add support for other
operating systems in the future.


## But how do you know it works?

The module has a small unit test suite, but it's nice to know test the
actual running of Puppet and installation of the software. For this I've
used [Test Kitchen](http://kitchen.ci/) and
[ServerSpec](http://serverspec.org/). This allows for spinning up 2
virtual machines (one for each supported operating system), applying the
Puppet manifest and then making some assertions:

{% gist a8d090b5d7f7a190f7d9 %}

The above is simply checking whether certain packages are installed, the
PPA is setup correctly and whether `mirage` and `opam` can be executed
cleanly.


## Can it produce a working unikernel?

The above tells us whether the installation worked, but not whether the
resulting software allows us to build MirageOS unikernels. For this I
used [Bats](https://github.com/sstephenson/bats) running in the same
Test Kitchen setup.

{% gist 191c4c0676b471f9b986 %}

The above configures and builds a simple HTTP server unikernel, and then
checks that when run it returns the expected response on the correct
port.


## Conclusion

I like the separation of concerns above. I can use the Puppet code
without the test code, or even swap the Puppet code out for a shell
script if I wanted. I could also run the serverspec tests anywhere I
want to check state, which is the reason for separating those tests from
the one's building and running a unikernel. Overall the tool chain for ad-hoc
infrastructure testing (quick mention of
[Infrataster](http://infrataster.net/) too) is really quite powerful and
approachable. I'd love to see more software ship with a user-facing test
suite for people to verify their installation works.
