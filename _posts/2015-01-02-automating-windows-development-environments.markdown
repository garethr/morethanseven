---
layout: post
title: "Automating windows development environments"
date: 2015-01-02
---

My job at Puppet Labs has given me an excuse to take a closer look at
the advancements in Windows automation, in particular [Chocolatey]( https://chocolatey.org/)
and [BoxStarter](http://boxstarter.org/). The following is very much a work
in progress but it's hopefully useful for a few things:

* If like me you've mainly been doing non-Windows development for a
while it's interesting to see what is possible
* If you're starting out with infrastructure development on Windows the
following could be a good starting place
* if you're an experienced Windows pro then you can let me know of any
improvements

All that's needed is to run the following from a CMD or Powershell
prompt on a new Windows machine (you can also visit the URL in Internet
Explorer if you prefer).

    START http://boxstarter.org/package/nr/url?https://gist.githubusercontent.com/garethr/a1838aa68355a0766de4/raw/d92b41ee9dcad68c079d24c64bac7d1d27cf37c7/garethr.ps1

This launches BoxStarter, which executes the following code:

{% gist a1838aa68355a0766de4 %}

This takes a while as it runs Windows update and repeatedly reboots the
machine. But once completed you'll have the listed software installed
and configured on a newly up-to-date Windows machine.
