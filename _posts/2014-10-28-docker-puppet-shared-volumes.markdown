---
layout: post
title: "Docker, Puppet and shared volumes"
date: 2014-10-28
---


During one of the openspace sessions at Devopsdays we talked about docker and configuration management,
and one of the things we touched on was using dockers shared volumes support. This is easier to explain
with an example.

First, lets create a docker image to run puppet. I'm also installing r10k for managing third party
modules.

## Docker

```
FROM ubuntu:trusty

RUN apt-get update -q
RUN apt-get install -qy wget
RUN wget http://apt.puppetlabs.com/puppetlabs-release-trusty.deb
RUN dpkg -i puppetlabs-release-trusty.deb
RUN apt-get update

RUN apt-get install -y puppet ruby1.9.3 build-essential git-core
RUN echo "gem: --no-ri --no-rdoc" > ~/.gemrc
RUN gem install r10k
```

Lets build that and tag it locally. Feel free to use whatever name you like here.

```
docker build -t garethr/puppet .
```

Lets now use that image as a base for another image.

```
FROM garethr/puppet

RUN mkdir /etc/shared
ADD Puppetfile /
RUN r10k puppetfile check
RUN r10k puppetfile install
ADD init.pp /
CMD ["puppet", "apply", "--modulepath=/modules", "/init.pp","--verbose", "--show_diff"]
```

This image will be used to create containers that we intend to run. Here we're including a
Puppetfile (a list of module dependencies) and then running r10k to download those dependencies.
Finally we add a simple puppetfile (this would likely be an entire manifests directory in most cases).
The final line means that when we run a container based on this image it will run puppet and then exit.

Again lets build the image and tag it.

```
docker build -t garethr/puppetshared .
```

Just as a demo, here's a sample `Puppetfile` which includes the puppetlabs stdlib module.

## Puppet

```
mod 'puppetlabs/stdlib'
```

And again as an example here's a simple puppet `init.pp` file. All we're doing is creating a file
at a specific location.

```
file { '/etc/shared/client':
  ensure => directory,
}

file { '/etc/shared/client/apache.conf':
  ensure  => present,
  content => "not a real config file",
}
```

## Fig

[Fig](http://fig.sh) is a tool to declare container types in a text file, and then run and manage
them from a simple CLI. We could do all this with straigh docker calls too.

```yaml
master:
  image: garethr/puppetshared
  volumes:
    - /etc/shared:/etc/shared:rw

client:
  image: ubuntu:trusty
  volumes:
    - /etc/shared/client:/etc/:ro
  command: '/bin/sh -c "while true; do echo hello world; sleep 1; done"'
```

The important part of the above is the volumes lines. What we're doing here is:

* Sharing the `/etc/shared` directory on the host with the container called master. The container will be able to write to the host filesystem.
* Sharing a subdirectory of of `/etc/shared` with the client container. The client can only read this information.

Note the client container here isn't running Puppet. Here it's just running sleep in a loop to simulate a long running process like
your custom application.

Let's run the master. Note that this will run puppet and then exit. But with the above manifest it will create
a config file on the host.

```
fig run master
```

Then run the client. This won't exit and should just print `hello world` to stdout.

```
fig run client
```

Docker 1.3 adds the handy exec command, which allows for one-off commands to be executed within a running container.
Lets use that to see our new config file.

```
docker exec puppetshared_client_run_1 cat /etc/apache.conf
```

This should output the contents of the file we created by running the master container.

## Why?

This is obviously a very simple example but I think it's interesting for a few reasons.

* We have completely separated our code (in the container) from the configuration
* We get to use familiar tools for managing the configuration in a familiar way

It also raises a few problems:

* The host needs to know what types of container are going to run on it, in order to have the correct configuration. If you're using [Puppet module](https://forge.puppetlabs.com/garethr/docker) then this is simple enough to solve.
* The host ends up with all of the configuration for all the containers in one place, you could also do things with encrypting the data and having the relevant keys in one image and not others. Given how if you're on the host you own the container anyway this isn't as odd as it sounds.
* We're just demonstrating files here, but if we change our manifest and rerun the puppet container then we change the config files. But depending on the application it  won't pick that up unless we restart it or create a new container.

Given enough time I may try build a reference implementation using this approach, anyone with ideas about that let me know.

This post was inspired by a conversation with [Kelsey](https://twitter.com/kelseyhightower) and [John](http://twitter.com/botchagalupe), thanks guys.


