---
layout: post
title:  "Unsupported sector size 520: reusing drives from a disk array by reformatting to a 512 block size"
date:   2019-01-16
categories:
---
You're probably reading this page because you came across a great deal on some SAS drives, and you can't get them to work.  As it turns out, many drives out of enterprise storage arrays have been formatted with non-standard block sizes, for various reasons.

Fortunately, there's a way to format them back to a standard 512 block size so they can be used in a typical server.

User ahouston on GitHub has posted a [fork](https://github.com/ahouston/setblocksize) of the great `setblocksize` tool which will send the correct commands to a SAS drive controller in order for it to reformat the drive to 512 block size.  This tool has been reported to work on IBM, Hitachi, and Seagate disks. Others are likely compatible as well.

Unfortunatly, the tool only reformats one drive at a time. I have a whole server full of these drives, so I made my own [fork](https://github.com/justinschmitt/setblocksize) which is capable of reformatting more than one drive at once.

Here's how to get started fixing your own drives:

# Prerequisites

Most importantly, you'll need an HBA or RAID controller capable of [IT mode](/2019/01/11/dell-perc200-it-mode.html) to connect your drives.  This is necessary because you'll need to have direct access to the drive in order to reformat it.  Some RAID controllers will not allow you to do this, but others can be reflashed to enable this type of access.

# Boot from an Ubuntu LiveCD image

Grab a flash drive, download [Ubuntu 18.04 LTS](https://www.ubuntu.com/download/desktop/thank-you?version=18.04.1&architecture=amd64) and install it to your flash drive with [Rufus](https://rufus.ie/) (or similar).  The rest of this guide has been designed to work with this version of Ubuntu, so pick this for best results.

Boot up your system from this flash drive.  I like to remove any drives that I _don't_ want to format before doing this.

# Verify that your drives have an incorrect block size

You don't want to do this if it's not the correct solution.

Open up a terminal and run `dmesg | grep 'sector size'`.  If you see messages in the terminal like this, then you know you've found the problem:
```
[   49.724403] sd 7:0:0:0: [sdb] Unsupported sector size 520.
```

# Get the required tools

Elevate to a superuser session in your terminal with `sudo bash`, and run the following script to get the required tools:
```
wget https://github.com/justinschmitt/setblocksize/raw/master/install.sh -O - | bash
```

This will install sg3-utils, download a modified version of setblocksize, and a script to enable looping through each drive to be reformatted.

# Identify the drives to reformat
Now you should be able to run `sg_scan -i`, which will return a list of drives attached to the system.  Identify the drives you need to reformat.

# Run the reformatting script
Now that you know which drives to reformat, pass them as arguments to `./multisetblocksize.sh`.

For _example_, to format all 4 drives from `/dev/sg0` through `/dev/sg3`:

```
./multisetblocksize.sh /dev/sg0 /dev/sg1 /dev/sg2 /dev/sg3
```

Once you initiate the process, you will get one final confirmation before formatting begins.

# And finally, wait

This script initiates a low-level format operation, which is being done by the drive controller on each drive.  In my experience, it takes about 1 hour per 450GB drive for this process to complete.  As each drive completes, the script will automatically move on to the next drive for formatting.
