---
layout: post
title:  "Windows context menu shortcuts to quickly merge files with Meld"
date:   2019-08-20
categories:
---
[Meld](http://meldmerge.org/) is a great cross-platform tool for merging files quickly and visually.  However, it requires several clicks to open files within its interface.  Luckily, there's an easy way to add Meld to your context menu, so that you can select two files and send them to Meld with your right-click context menu.

1.  Open the 'Send To' shortcut folder by opening the Run window with `WIN + R`, and then enter `shell:sendto`
2.  Within the folder that opens, simply add a shortcut to your Meld executable, likely `C:\Program Files (x86)\Meld\Meld.exe`.
3.  You should now see Meld within your right click context menu, under the 'Send To' submenu!