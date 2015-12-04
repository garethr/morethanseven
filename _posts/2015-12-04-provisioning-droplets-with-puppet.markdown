---
layout: post
title: "Provisioning droplets with Puppet"
date: 2015-12-04
---


I love [DigitalOcean](https://www.digitalocean.com/) for quickly spinning
up machines. I also like managing my infrastructure using Puppet. Enter the
[garethr-digitalocean](https://forge.puppetlabs.com/garethr/digitalocean) module.
This currently provides a single Puppet type; `droplet`.

Lets show a quick example of that, by launching two droplets, called
test-digitalocean and test-digitalocean-1.

```puppet
droplet { ['test-digitalocean', 'test-digitalocean-1']:
  ensure => present,
  region => 'lon1',
  size   => '512mb',
  image  => 14169855,
}
```

With the above manifest saved as `droplets.pp` we can run it with:

```
$ puppet apply --test droplets,pp
```

This will ensure those two droplets exist in that region, and have that
size. If they don't exist it will launch droplets using the specified image.
This means we can run the same command again, and rather that create
more instances it will simply report that we currently have those
droplets already.

## Querying resources

Puppet also comes with `puppet resource`, a handy way of querying the
state of a given resource or type. Running the following will list all
of your droplets, whether you created them using Puppet or not.

```
$ puppet resource droplet
droplet { 'test-digitalocean':
  ensure              => 'present',
  backups             => 'false',
  image               => '14169855',
  image_slug          => 'ubuntu-15-10-x64',
  ipv6                => 'true',
  price_monthly       => '10.0',
  private_address     => '10.131.98.186',
  private_networking  => 'true',
  public_address      => '178.62.25.100',
  public_address_ipv6 => '2A03:B0C0:0001:00D0:0000:0000:0090:B001',
  region              => 'lon1',
  size                => '1gb',
}
```

## Mutating resources

The type also supports mutating droplets, for instance changing the
size of a droplet if you change the model in Puppet. The API client
doesn't support all possible changes, but you can disable backups, enable
IPv6 and switch on private networking as needed. Here's a quick sample
of the output showing this in action.

```
Info: Loading facts
Notice: Compiled catalog for gareths-macbook.local in environment production in 0.43 seconds
Info: Applying configuration version '1449225401'
Info: Checking if droplet test-digitalocean exists
Info: Powering off droplet test-digitalocean
Info: Resizing droplet test-digitalocean
Info: Powering up droplet test-digitalocean
Notice: /Stage[main]/Main/Droplet[test-digitalocean]/size: size changed '1gb' to '512mb'
Error: Disabling IPv6 for test-digitalocean is not supported
Error: /Stage[main]/Main/Droplet[test-digitalocean]/ipv6: change from true to false failed: Disabling IPv6 for test-digitalocean is not supported
Error: Disabling private networking for test-digitalocean is not supported
Error: /Stage[main]/Main/Droplet[test-digitalocean]/private_networking: change from true to false failed: Disabling private networking for test-digitalocean is not supported
Info: Checking if droplet test-digitalocean-1 exists
Info: Created new droplet called test-digitalocean-1
Notice: /Stage[main]/Main/Droplet[test-digitalocean-1]/ensure: created
Info: Class[Main]: Unscheduling all events on Class[Main]
Notice: Applied catalog in 60.61 seconds
```

## But why?

Describing your infrastructure at this level in code has several advantages:

* Having a shared model of your infrastructure in code allows for a discussion
  around that model
* You can be convident in the model because of the idempotent nature of running
  the code
* The use of code for this model allows for activities like code review, change
  control based on pull requests, unit testing, user created abstrations and more
* The use of Puppet means you can use it as above as a command line interface, or
  run it every period of time to enfore and report on the state of you infrastructure
* Puppet ecosystem tools like PuppetDB, Puppet Board or Puppet Enterprise mean you can
  store data over time for later analysis

The module also acts as a reasonable example of a simple Puppet type and provider.
If you're interested in extending Puppet for your own services this is hopefully a
good place to start understanding the API.
