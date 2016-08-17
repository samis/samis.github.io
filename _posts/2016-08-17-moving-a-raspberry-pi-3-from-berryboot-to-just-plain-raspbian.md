---
layout: post
title: "Moving a Raspberry Pi 3 from Berryboot to just plain Raspbian"
description: ""
category: 
tags: []
---
{% include JB/setup %}
For a while now, I've had a Raspberry Pi 3, replacing my original Pi. The Pi, inside of my chosen case looks something like the below image

<img src="https://cdn.shopify.com/s/files/1/0174/1800/products/Rainbow_1_of_3_47e94e82-ba3a-4804-a280-3140109cd304_1024x1024.jpg?v=1456669057" alt="Raspberry PI 3 with Pibow case" width="200" height="200"/>

Originally, I thought that it'd be cool to be able to install/uninstall/update multiple distros on it. NOOBS can do this but I believe you can't do it while retaining existing data whenever you add/remove OSes / distributions. Instead, I became aware of (and chose) Berryboot instead. It provided a decent boot menu to select what you wanted to boot from while enabling you to add/remove new items without affecting existed installed ones. It did this by not giving each item it's own partition - instead, it stored the initial download as a filesystem image and used AUFS to persist any user-made changes to the downloaded system. 

As time passed, I never actually used this functionality - my Pi 3 always booted Raspbian, I never bothered to even *install* anything else, never mind use/boot it. I continued to use Berryboot, even if I didn't really need it and would do just fine with a simple plain Raspbian install because it caused no issues (that I noticed anyway).

One day, the time came to reboot my Pi. I had done this multiple times before without any issues. However, on this attempt all I got after the reboot was that 4-pixel rainbow screen stuck there. I did some googling / research on this problem led to me to [this](https://github.com/maxnet/berryboot/issues/293) GitHub issue. It says that after upgrading the installed OS, a reboot may cause the  exact same symptoms that I saw. 

I had two options:

* Replace the problem Berryboot files with copies from the installed OS.
* Somehow get rid of Berryboot and boot Raspbian directly...while preserving the exact state and data of my install of Raspbian.
I chose the second option, reasoning that it'd be simpler and possibly more performant too (NO use of AUFS, just direct writes to the underlying media.)

Now that I had chosen to remove Berryboot, I had to face the problem of migrating all my data/configuration. Since all my modified data was just a directory on a partition , I couldn't simply use `dd` to take a copy and place it back after removing Berryboot. I also couldn't simply create a blank partition and copy the existing data into it - only the modifications were stored as normal files/directories, and it was practically certain that some files had not been modified and as such would be missing.

I came up with a plan that would (hopefully) work, and should anyone else need to do this, the steps are below:

1. Create a tarball of the filesystem (Compression is optional, but you likely want to) - Make it's not on the SD card itself because it will be erased in the next step.
2. Download (and extract) the latest release of Raspbian. Use `dd ` (or whatever tool is appropriate) to write the resulting disk image to the SD card. Be **very careful** when using `dd` because it's very easy to overwrite the wrong partition and lose data. 
3. Extract the tarball onto the root filesystem of your Raspbian. All files that were on your original installation will be on this one too, while any missing/absent files will remain from the freshly-installed Raspbian.

 It took a while, but I successfully performed all of these 3 steps (I also took the opportunity to make good use of GParted and resize the root filesystem before the first boot). The resulting system successfully booted and launched all the services that were previously running. However it was not an exact copy - The SSH keys changed for example.
