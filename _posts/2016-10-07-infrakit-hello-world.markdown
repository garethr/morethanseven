---
layout: post
title: "InfraKit Hello World"
date: 2016-10-07
---

Docker just shipped [InfraKit](https://github.com/docker/infrakit) a few days ago at LinuxCon and, while at the Docker Distributed Systems Summit, I wanted to see if I could get a hello world example up and running. The documentation is lacking at the moment, epecially around how to tie the different components like instances and flavors together.

The following example isn't going to do anything particularly useful, but it's hopefully simple enough to help anyone else trying to get started. I'm assuming you've checked out and built the binaries as described in the [README](https://github.com/docker/infrakit#building).

First create a directory. We're going to be using InfraKit to manage local files in that directory as part of the demo.

```
mkdir test
```

Now create an InfraKit configuration file. We're going to use the `file` instance plugin to manage files in out directory. This means everything works on the local machine, rather than trying to launch real infrastructure in AWS or similar. InfraKit also requires a `flavor` plugin. I'm using `vanilla` here just to meet the requirement for a flavor plugin, but it's not going to actually do anything in this demo. It might be useful to write a noop flavor plugin or similar.

```
cat garethr.json
```

```json
{
    "ID": "garethr",
    "Properties": {
        "Instance" : {
            "Plugin": "instance-file",
            "Properties": {
            }
        },
        "Flavor" : {
            "Plugin": "flavor-vanilla",
            "Properties": {
                "Size": 1
            }
        }
    }
}
```

InfraKit is based on running separate plugins. Each plugin runs as a separate process and provides a filesystem socket in /run/infrakit/plugins. First start up the file plugin:

```
$ ./infrakit/file --dir=./test
INFO[0000] Starting plugin
INFO[0000] Listening on: unix:///run/infrakit/plugins/instance-file.sock
INFO[0000] listener protocol= unix addr= /run/infrakit/plugins/instance-file.sock err= <nil>
```

Next, in a separate terminal run the vanilla plugin:

```
$ ./infrakit/vanilla
INFO[0000] Starting plugin
INFO[0000] Listening on: unix:///run/infrakit/plugins/flavor-vanilla.sock
INFO[0000] listener protocol= unix addr= /run/infrakit/plugins/flavor-vanilla.sock err= <nil>
```

An finally run the group plugin. I'm passing `--log=5` to enable more verbose outout so it's easier to see what's going on with the group.

```
$ ./infrakit/group --log=5
INFO[0000] Starting discovery
DEBU[0000] Opening: /run/infrakit/plugins
DEBU[0000] Discovered plugin at unix:///run/infrakit/plugins/instance-file.sock
INFO[0000] Starting plugin
INFO[0000] Starting
INFO[0000] Listening on: unix:///run/infrakit/plugins/group.sock
INFO[0000] listener protocol= unix addr= /run/infrakit/plugins/group.sock err= <nil>
```

With that all setup we can create a group based on our configuration file from above.

```
$ ./infrakit/cli group --name group watch garethr.json
watching garethr
```

Have a look in the test directory. You should see a single file has been created.

```
$ ls test
instance-1475833380
```

Let's delete that file and see what happens:

```
rm test/*
```

Hopefully InfraKit will spot the instance (a file in this case) no longer exists and recreate it. You should see something like the following in the logs:

```
INFO[0612] Created instance instance-1475833820 with tags map[infrakit.config_sha:B2MsacXz8V_ztsjAzu3tu3zivlw= infrakit.group:garethr]
```

This is obviously a less-than-useful example but hopefully provides a good hello world example for anyone trying to run InfraKit in it's current early stage.
