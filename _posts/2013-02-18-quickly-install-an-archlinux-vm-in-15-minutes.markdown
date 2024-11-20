---
title: Quickly Install an ArchLinux VM in 15 minutes
date: 2013-02-18
layout: single
---

Today we'll be looking at getting ArchLinux up and running in
VirtualBox as rapidly as possible.~

**Prerequisites:**

1. You have download the latest ArchLinux image from the [downloads
   page][].
2. You have [VirtualBox][] installed and up to date.
3. You have some basic Linux hacking skills.
4. You're fine with US English and keymap. The [installation guide][]
   and [beginner's guide][] are great references if you need to change
   them.

Let's do it!

* Make a new VM; select "Linux" and "Arch Linux" and a sane amount of
  RAM (we'll be going with no swap space) and disk space. Start it up
  and select the Arch Linux image you downloaded. Boot into the
  appropriate kernel depending if you want 64- or 32-bit.
* Now the fun begins - we're doing the rest in the shell with comments
  as commentary. You can download this directly by doing
  `curl -L http://bit.ly/archlinux-install >install.sh`

<script src="https://gist.github.com/wuputah/4982514.js"></script>

* Login as `root` with password `root` and begin customizing your
  system. I plan on making a script for this as well, but for now, the
  basics are:

  * Change your root password with `passwd`.
  * Add a user using [useradd][]
  * Install packages with `pacman -S`, search with `pacman -Ss`. [Here's
    some suggestions on what to install][extras] and [more about
    pacman][pacman].
  * Tell daemons to run at bootup by doing `systemctl enable`, start
    them once using `systemctl start`. [Learn more about systemctl and
    systemd][systemd].
  * Get VirtualBox support for resizing etc by following the [VirtualBox
    article][vbox].
  * Things that would work out of the box in Ubuntu often require
    software installation and a bit of configuration.  This is usually
    straightforward, but requires a bit of reading on the ArchLinux
    wiki.

[downloads page]: https://www.archlinux.org/download/
[VirtualBox]: https://www.virtualbox.org/wiki/Downloads
[beginner's guide]: https://wiki.archlinux.org/index.php/Beginners%27_Guide
[installation guide]: https://wiki.archlinux.org/index.php/Installation_Guide
[useradd]: https://wiki.archlinux.org/index.php/Users_and_Groups#User_management
[extras]: https://wiki.archlinux.org/index.php/Beginners%27_Guide/Extra
[pacman]: https://wiki.archlinux.org/index.php/Pacman
[systemd]: https://wiki.archlinux.org/index.php/Systemd#Basic_systemctl_usage
[vbox]: https://wiki.archlinux.org/index.php/VirtualBox
