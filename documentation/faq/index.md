---
layout: affix
title: FAQ
---

Frequently asked questions... If you don't find the answer here, it's either in
the [Users Guide](/documentation/usersguide), the [Developers Guide](/documentation/developers)
or not yet written. For the latter, see [](#)

### General

#### Why A Fork?

Read [Why A Fork](/project.html) on the projects page.


#### How Can I Help?

Help is much appreciated and possible in many ways. Details can be found on
the [community page](/community) page.

### Suite

### Core

#### Run Naemon With GDB

For error tracing its often useful to have a backtrace. For example if naemon
crashes. You can start naemon with GDB like this.

<div class="alert alert-warning"><i class="glyphicon glyphicon-exclamation-sign"></i> All commands should be run as the 'naemon' user.</div>

Livestatus requires us to export or set LD_PRELOAD before running GDB, so we
first have to find that library:

```bash
  %> find /lib/ /usr/lib/ /lib64/ /usr/lib64/ -name libpthread.so.0
  /lib/i386-linux-gnu/libpthread.so.0
```

Then run Naemon with GDB, you will have to type 'run' after the prompt:

```bash
 %>LD_PRELOAD=/lib/i386-linux-gnu/libpthread.so.0 gdb --args /usr/bin/naemon /etc/naemon/naemon.cfg
   GNU gdb (GDB) 7.4.1-debian
   Copyright (C) 2012 Free Software Foundation, Inc.
   License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
   This is free software: you are free to change and redistribute it.
   There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
   and "show warranty" for details.
   This GDB was configured as "i486-linux-gnu".
   For bug reporting instructions, please see:
   <http://www.gnu.org/software/gdb/bugs/>...
   Reading symbols from /usr/bin/naemon...(no debugging symbols found)...done.
   (gdb) run
```

After typing 'run' you need to wait till the program crashes or exists otherwise.
In our case we just killed the core, so we will get this message:

```bash
Program received signal SIGTERM, Terminated.
0xb7fe1424 in __kernel_vsyscall ()
(gdb) bt
```

By typing 'bt' you will get the desired backtrace:

```
#0  0xb7fe1424 in __kernel_vsyscall ()
#1  0xb7f061f6 in epoll_wait () from /lib/i386-linux-gnu/i686/cmov/libc.so.6
#2  0x080c38b7 in iobroker_poll ()
#3  0x08079f2d in event_execution_loop ()
#4  0x0805ae62 in main ()
```

Now go to the [naemon issues](https://github.com/naemon/naemon/issues) page and file a
new bug after having a look if this hasn't been reported yet.


### Thruk

### Livestatus

### Addons

### Development

### Documentation

#### Help Extending The Documentation

Helping writing documentation is really easy and much appreciated. Most pages
are written in either Markdown ([Cheat Sheet][markdown]) or HTML. The website
itself uses [bootstrap][bootstrap] for a simple and nice layout.

##### Small Changes
Small changes like typos can be corrected and submited directly on [github][githubdocs] via the edit button.
<p>![github edit button](images/githubedit.png "Github Edit Button")</p>
Just navigation to the page you want to change and send a pull request via the online editor of github.
Read more about the [online editor][githubedithelp]...


##### Larger Changes
Larger changes should be tested and reviewed locally before submiting them. Also
it's good practice to talk to a team member before spending large amounts of time
in things we eventually won't accept for whatever reasons.
The [developer guide](/documentation/developer/#documentation) contains instructions on
how to run a local Jekyll server.
When done, just submit a normal pull request.


[markdown]: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet
[bootstrap]: http://getbootstrap.com/css/
[githubdocs]: https://github.com/naemon/naemon.github.io/tree/master/documentation
[githubedithelp]: https://github.com/blog/905-edit-like-an-ace