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

I have been tracking my dotfiles for short-to-moderate period of time. I originally started when I read an article about using GNU Stow as the management tool.
Stow has some features that make it just as useful for this as a dedicated too: It supports 'packages' so you can choose to install part of the dotfiles.
It also doesn't make you specify specifically which files to symlink, it just symlinks the entire package.
However, it's definitely not perfect: Symlinks can be overwritten, Moving dotfiles and replicating directory structures sucked, and you could only manage operations from the right directory.
(I could also only easily have 1 VCS repo, which effectively meant private dotfiles couldn't be tracked)

One day, while inspecting my ~/dotfiles I noticed that the .git directory was missing. I could've seen this as a disaster, but I didn't.
I had been thinking about migrating away from Stow for a while, but I never actually did anything - so I took this opportunity.
After some reading/googling, I had made the decision to use `mr` and `vcsh`.
`vcsh` would provide each individual repository, public and private while `mr` would be used for higher-level tasks.
There are multiple guides to this pair of tools, such as:

  * [This very short post on vcsh & mr](https://sumancluster.wordpress.com/2015/05/29/managing-dotfiles-using-vcsh-and-mr/)
  * [This one which  links to a more in-depth tutorial but also takes a look at the internals](https://www.kunxi.org/blog/2014/02/manage-dotfiles-using-vcsh-and-mr/)
  * [This very useful and detailed one](http://srijanshetty.in/technical/vcsh-mr-dotfiles-nirvana/)

When I was migrating, I particularly found the latter link to be rather useful due to the detailed explanations of multiple common tasks.
However, should you not want to read any of the above links I will attempt to give an overview of how it works in practice.

# Creating a new repository

1. Clone/Initialize the local vcsh repository
2. Update the myrepos(mr) configuration to include that repository
3. Add the wanted stuff to the vcsh repository
4. Write/generate a .gitignore and modify as needed
5. Commit to the vcsh repository and push both sets of changes as needed.

# Updating an existing repository

1. You can prefix git operations with vcsh and then the repo name to perform them on the repository.
2. Alternatively, use 'vcsh enter' to go into an environment where git can be used normally.

# Updating *all* the repositories 

1. Use `mr up` and let myrepos do the job it was designed to do.

# Bootstrapping the dotfiles
(presuming git is installed. If not, install it.)

1. Install myrepos and vcsh. If there's no distribution package, a manual install is easy (they're just standalone scripts)
2. Obtain your myrepos configuration.
3. Use `mr up` and let myrepos obtain all your repositories as needed.

If you think the above workflow looks interesting, I recommend you have a nice read of the above links - especially the last one
as I found it very useful. However, I have not moved my entire collection of dotfiles over and yet I still have some interesting problems/caveats to tackle.

Firstly, while using a (private) Git repository to track my SSH/GPG data is useful, certain files have special filesystem permissions which Git does not preserve. While this can be solved with a chmod or two, it may grow
to be more difficult if I need more of these files in the future - plus I might be able to automate it using mr's 'fixups' functionality.

Secondly, this is more of an observation than a problem: I'm currently using an Apache-style configuration involving both *'available.d'* and *'config.d'*. This works and is flexible, but it'd be simpler to only have a single directory and have equivalent information be part of the configuration itself.

Thirdly, bootstrapping from a completely clean slate is rather complicated. Certain repositories may depend on others to work / be in the correct location. Then there's the problem of access to private repositories, perhaps HTTP(s) could be used to download SSH keys using pre-entered cached credentials? A similar but lesser problem exists with GPG. Public repositories have no issues with this - if need be, they can have the master remote be changed afterwards.s

Anyway, that's all for now. If and when I solve the above issues I'll make sure to explain and blog about each my solutions. Until then, I don't expect this to come up again.
