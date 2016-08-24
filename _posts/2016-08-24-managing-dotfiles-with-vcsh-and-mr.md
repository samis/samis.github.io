---
layout: post
title: "Managing dotfiles with vcsh and mr"
description: ""
category: 
tags: []
---
{% include JB/setup %}
Over time, a Linux user may customize and configure his environment rather substantially.
These modifications are stored in a collection of configuration files / data known as 'dotfiles' (because the first letter of many of them is '.').
For multiple reasons, it is very beneificial if you track, control and synchronise all of your personal dotfiles, a few example reasons include:
- Having an additional backup
- Being able to see their history, how they changed over time
- Being able to rollback changes if needed (I haven't needed this yet)
- Being able to use the same set of files accross multiple physical/virtual machines
- Being able to share you configuration with the world so people can learn from it just like you learn from other people's.

However, there is not one single universal method for managing them, instead there are many tools and approaches that one can take.
GitHub provides a decent list of programs [here](https://dotfiles.github.io/) but I intend to summarize the main approaches below.
(It may be worth noting that while the methods may not be mutually exclusive, there is one 'main' approach/method per tool and that is what counts.)

1. Symlink-driven management involves moving the dotfiles away from their original location, instead creating symbolic links to one or more destinations.
   There are many ways/approaches of doing this, but the simplest is to just have a single directory be the destination for all the links.
2. VC(Version Control)-driven management involves less management of actual dotfiles compared to the other two. Instead of copying or using symbolic links,
   instead a version-control system is primarily used to track/manage dotfiles in groups. The original dotfiles are left in place, instead they can be treated 
   just like every other repository. There are multiple methods of implementing this approach with their own unique advantages and drawbacks.
3. Configuration-driven management involves using explicit configuration file(s) to determine exactly what dotfiles are to be managed/tracked as well as how they are to be tracked among other things.
   The key difference between this method and the others is that rather than using interactive commands to manage and modify dotfiles, one or more configuration files are used. 
   Typical formats for this information include YAML/JSON or a purpose-built configuration format. Typically but not exclusively uses symbolic links for dotfiles.
4.
