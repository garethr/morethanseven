---
layout: post
title: "Using Puppet with key/value config stores"
date: 2014-10-12
---

I like the central idea behind storing configuration in something like
[Etcd](https://github.com/coreos/etcd) rather than lots of files on lots
of disks, but a few challenges still remain. Things that spring to mind
are:

* Are all your passwords now available to all of your nodes?
* How do I know when configuration changed and who changed it?

I'll leave the first of those for today (although have a look at
[Conjur](http://www.conjur.net/) as one approach to this). For the second,
I'm quite fond of plain text, pull requests and a well tested deployment
pipeline. Before Etcd (or [Consul](http://www.consul.io/) or similar) you
would probably have values in Hiera or Data Bags or similar and inject them
into files on hosts using your configuration management tool of choice. So
lets just do the same with our new-fangled distributed configuration store.

```puppet
key_value_config { '/foo':
  ensure   => present,
  provider => etcd,
  value    => 'bar',
}
```

Say you wanted to switch over to using Consul instead? Just switch the provider.

```puppet
key_value_config { '/foo':
  ensure   => present,
  provider => consul,
  value    => 'bar',
}
```

You'd probably move all of that out into something like hiera, and then generate
the above resources, but you get the idea.

```yaml
etcd_values:
  foo: bar
```

The above is implemented in a very simple
[proof of concept Puppet module](https://github.com/garethr/garethr-key_value_config).
Anyone with any feedback please do let me know.
