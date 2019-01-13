---
layout: post
title:  "Flashing a Dell PERC H200 to IT mode with an R710"
date:   2019-01-11
categories: perc r710 hardware
---
This guide will outline the steps to flash a PERC H200 with LSI-9211-8i firmware to IT mode to allow direct disk access on an R710. This is helpful for situations where direct disk access is preferred over a RAID configuration, for instance, for use with a FreeNAS installation.

There’s a guide techmattr put together [here](https://techmattr.wordpress.com/2016/04/11/updated-sas-hba-crossflashing-or-flashing-to-it-mode-dell-perc-h200-and-h310/) that covers most of these steps, but I found it was missing a couple of details. These steps should ensure that your drives remain available for use as a BIOS boot device.

The steps to complete are:

# Prepare boot media:
1. Get yourself a USB flash drive, just about any size will do.
2. Download [Rufus](https://rufus.ie/) to prepare the drive for use as FreeDOS boot media.
3. Open Rufus, select your flash drive, choose FreeDOS as your boot selection, and click START

# Download firmware:
1. Techmattr has complied most of the files you will need, (and more) at the link [here](https://www.mediafire.com/download/6mtie10d9ud6675/LSI-9211-8i.zip). Simply extract this zip to the root of your flash drive.
2. Additionally, to preserve the functionality to boot from your drives, you’ll need the LSI-9211-8i BIOS rom, available [here](https://docs.broadcom.com/docs/12350530). Under the `sasbios_rel` directory, you’ll find `mptsas2.rom`. Copy this file to the root of your flash drive.

# Flash the firmware: 
Insert your flash drive in your R710, and boot from it. At the DOS prompt, run the following commands:
1. `sas2flsh.exe -c 0 -list`
Make a note of the SAS Address listed.
2. `megarec.exe -writesbr 0 sbrempty.bin`
3. `megarec.exe -cleanflash 0`
When this is complete, reboot your system!
4. `sas2flsh.exe -o -f 6GBPSAS.fw`
When compete, reboot again.
5. `sas2flsh.exe -o -f 2118it.bin -b mptsas2.rom`
6. `s2fp19.exe -o -sasadd YOUR_SAS_ADDRESS`
replace YOUR_SAS_ADDRESS with the actual SAS Address value you wrote down previously. Omit any dashes.

# Remove the flash drive, reboot, and you’re done!

