# How User Space Starts

User space starts in roughly this order:

1. init
2. Essential low-level services such as udevd and syslogd
3. Network configuration112 Chapter 6
4. Mid- and high-level services (cron, printing, and so on)
5. Login prompts, GUIs, and other high-level applications

## Introduction to `init`

The init program is a user-space program like any other program on the Linux system, and you’ll find it in /sbin along with many of the other system binaries. __Its main purpose is to start and stop the essential service processes on the system__, but newer versions have more responsibilities.

Differen `init` implementations:

1. **System V** init A traditional sequenced init (Sys V, usually pronounced “sys-five”). Red Hat Enterprise Linux and several other distributions use this version.
2. **systemd** The emerging standard for init. Many distributions have moved to systemd, and most that have not yet done so are planning to move to it.
3. **openrc** ...
---
- There are various other versions of init as well, especially on embedded platforms. For example, Android has its own init. The BSDs also have their version of init, but you are unlikely to see them on a modern Linux machine.


- There are many different implementations of init because System V init and other older versions relied on a sequence that performed only one startup task at a time. Under this scheme, it is relatively easy to resolve dependencies. However, performance isn’t terribly good, because two parts of the boot sequence cannot normally run at once. 

- Another limitation is that you can only start a fixed set of services as defined by the boot sequence: When you plug in new hardware or need a service that isn’t already running, there is no standardized way to coordinate the new components with init.

### Diff Between `systemd` and `upstart`

- **systemd** is __goal oriented__. You define a target that you want to achieve, along with its dependencies, and when you want to reach the target. systemd satisfies the dependencies and resolves the target. systemd can also defer the start of a service until it is absolutely needed.
- **Upstart** is __reactionary__. It receives events and, based on those events, runs jobs that can in turn produce more events, causing Upstart to run more jobs, and so on.