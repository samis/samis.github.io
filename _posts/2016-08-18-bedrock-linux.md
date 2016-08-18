---
layout: post
title: "Bedrock Linux"
description: ""
category: 
tags: []
---
{% include JB/setup %}
I am very much a fan of Linux, using it as my primary OS on my computer. Obviously, I have used multiple distributions of it.
Each distribution has it's own independent software library that is integrated with the package manager and the system as a whole.
(Note: I am very much aware that Linux From Scratch and similar exists. I'm talking about the general case where *some* form of package manager/management exists. )

This has some advantages:

- No random downloading of installers/executables from the Internet like on Windows
- You can browse and search for available software
- Everything in the repositories follows a single set of standards / policies that the user can apply to any installed program.

All in all, it's a very wonderful user experience. However, it isn't perfect. Repositories provided are always finite.
They cannot and will not include every program that exists, nor include variations of included programs.
This can very easily become a problem, such as in the following situations:

- You want a different version of the program than the one available in the repositories.
- You want a program that simply isn't in the repositories.
- You want a program that is in the repositories..but was created using options you want to change.

If you enter this situation, there are many many ways to manage/deal with it, each having their own trade-offs/side-effects
but today I'm going to focus on one particular case:
You are a user on Distro X that has somehow got into one of the 3 situations described above.
While browsing the internet for solutions, you see that a package from Distro Y would get you out of this situation.
How do you install that package from Distro Y onto your Distro X installation?

Normally, you simply *can't*. Distro Y packages are built to work on that one only, there's no support for Distro X
and you can't even install it, since Distro X's package manager only supports the specific format used by Distro X.
Even if you did get it to install, you'd have problems with dependencies and other cross-distro differences.

At this point you might be asking, 'What is Bedrock Linux and how does it come into this' to which I answer this:
Bedrock Linux allows you to *combine* multiple installed distributions. You're not limited to just 'Arch Linux' or just 'Debian'.
Instead, you can have both Arch and Debian installed and be using programs with each concurrently. Of course, those two are just examples
- you can have any number of distros concurrently installed and functioning.

It should be obvious how this applies to the hypothetical situation above. For someone using Bedrock Linux, the above is mostly a non-issue
as packages from Distro Y can easily be installed - even if most of the packages on your system come from Distro X.
The full story of how this is achieved is somewhat complex and involves decent amounts of filesystem manipulation but to simplify, each
distribution/chunk of files is called a _stratum_ in Bedrock Linux terms. Aside from special strata, each stratum is a self-contained installation of a distribution. 
The combination of multiple strata as a single system results in something that not only has a much deeper pool of software to draw upon and use, but can leverage the strengths provided by each individual stratum.

Under Bedrock Linux, you can install Distro Y packages on a mostly-Distro-X system because that Distro Y package is installed into a complete functional installation of Distro Y.
(and is accessible via a filesystem directory specially maintained by a Bedrock Linux component)
There are certainly many other potential applications and use cases for Bedrock Linux, but this is one of the more obvious and immmediately useful ones.

Should you wish to find out more, there's plenty of documentation [here](https://bedrocklinux.org/).
