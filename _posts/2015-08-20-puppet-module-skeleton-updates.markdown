---
layout: post
title: "Update to Puppet Module Skeleton"
date: 2015-08-20
---


Being on holiday last week meant I had a little time for some gardening
of open source projects and I decided to update
[puppet-module-skeleton](https://www.github.com/garethr/puppet-module-skeleton)
with some new opinions.

The skeleton is a replacement for the default module skeleton that ships
with Puppet and is used by puppet module generate. Unlike the default
skeleton this one is super-opinionated. It comes bundled with lots of
testing tools, suggestions for documentation, integration with [Travis
CI](http://travis-ci.org), module coverage reports and more.

Updates in the latest version include:

* Support for Puppet 4 paths
* The addition of Rubocop, which enforces parts of the Ruby style guide
* Adding a number of Puppet Lint plugins
* Allow installing various Puppet versions during integration tests

I also fixed a few reported bugs and extended the test matrix to test
across a range of Puppet and Ruby combinations.

The skeleton is intended to help people with a basic understanding of
Puppet write better modules, without having to setup everything
themselves. You don’t have to agree with all the options to make use of
the skeleton as it’s simple enough to delete a few files once you
generate your new module. But a working out-of-the-box beaker install,
and the ability to automatically run unit tests when files change are
patterns worth adopting for most module developers I think.

If anyone has any suggestions for extra tools, or changes to the
skeleton itself, let me know.

