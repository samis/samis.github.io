---
layout: post
title: "How I almost sucessfully installed Gentoo Linux (and very brief opinions on distributions)"
description: ""
category: 
tags: []
---
{% include JB/setup %}
I'm not a distro-hopper by any means, but even so I have tested/tasted a number of Linux distributions. 
Primarily, these have been in the Debian family: X/K/Ubuntu, Debian itself, Raspbian and likely more that I'm forgetting.
I was recommended and used very happily for a good while Arch Linux (experiments with GNU Guix on it notwithstanding), until my hard drive began dying one day.
I had heard that Tumbleweed was also rolling-release, and provided interesting rollback functionality out of the box using BTRFS and Snapper, so I installed it on a spare USB stick.
Recently, I was thinking about Gentoo Linux. I was mainly thinking of the perhaps not-entirely-accurate idea that it would take substantial time to install due to the requisite amounts of compiling
I also thought that the difficulty level of the installation was roughly equivalent to that of Arch,. I wanted to see if my thoughts/perceptions were right, so I planned to install and migrate to Gentoo.
This led to a sequence of events that can be divided into approximately 3 parts.

# Part I: The actual installation of Gentoo
Much like Arch Linux, Gentoo has a [comprehensive wiki]() filled with documentation about not only the installation procedures but also a large number of other things that are needed post-install. 
This is a very good thing, because documentation is very useful when installing either distribution (especially if you haven't done it before).
As such, I mostly ended up following the [Gentoo Handbook]() which provides a well-written resource much like Arch's own installation guide (except it seemed more organized and structured into steps).
Seeing as I was going to install Gentoo onto an existing filesystem (as a BTRFS subvolume) and was installing from an existing Linux rather than a CD, I could ignore 3 segments of the first part.
The remaining installation steps looked like this:

1. Download (and extract) a precompiled base system (a stage3 tarball)
This stage was very easy, only a couple of commands to execute with no decisions to make.
2. Set appropriate compiliation settings
At this point I needed to select what compiliation flags I would be using as well as decide how many parallel jobs `make` should be running.
I decided to go with the default set of flags, only tweaking it to target GCC towards my specific CPU type (`-march=amdfam10`) and to also follow the recommendation for job count so that `make` could run up to 5 tasks in parallel.
This was a very good decision - for one thing it made sure that compiling felt very fast and also ensured that all of my CPU's capacity could be used by the process if needed. 
3. Enter the installed base system and configure/update Portage (Gentoo's package manager)
This step was also rather easy, a bit of copying files around and a few commands. I selected the generic 'desktop' profile, not seeing one more accurate.
4. Rebuild the world
Now that I had selected my profile, I needed to update my system to include the changed settings/flags that came with the new profile.
Additionally, I needed to install the additional software selected to my profile. 
In short, what I (or Gentoo's portage) actually did could be succinctly explained with this image:
![COMPILE ALL THE THINGS](https://cdn.meme.am/instances/500x/71652744.jpg)
I expected that this would be the longest part of the installation, and that was a correct expectation. Compiling 164 packages does take some time.
However, it didn't take as much time as I imagined it to, things felt pretty fast actually. Building a generic linux kernel from scratch and installing it only took ~1h. 
I attribute this unexpected speediness to the benefits of passing `-j5` to make - Allowing 4 files to be compiled at once while using an entire CPU core speeded things up very nicely, while a 5th task meant there was almost always something to do when it was otherwise idle.
5. Configuration of USE flags/locale/timezone
At the present time, I decided to not really touch the USE flags immediately as they could be easily modified later as an when I needed to.
I set the locale & timezone in accordance with my physical location (the UK).
6. Compiling and installation of the kernel
I decided that rather than start with a custom kernel configuration that may or may not boot, I would instead start with Genkernel, which would provide me a base from which to customise my own kernel.
Considering that the result was a rather generic kernel, it was a bit surprising that it only took an hour or so to compile and install the kernel from scratch.
7. General system configuration
In this stage, I wrote /etc/fstab as well as configuring the network (simply automatically running DHCP on the only ethernet interface).
I also assigned the system a hostname, and made sure that OpenRC used the correct keymap and started the network at boot-time.
Before moving on to bootloader configuration, I selected what initial optional services I wanted installed and running at boot. These included a system logger, a cron daemon as well as `mlocate`.

The next stage was bootloader configuration, but I think discussion of that would fit better in Part II. This post is getting somewhat long, so that'll be in another post in a short while.
