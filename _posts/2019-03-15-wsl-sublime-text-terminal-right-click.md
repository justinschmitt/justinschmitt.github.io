---
layout: post
title:  "Open a Windows Subsystem for Linux (WSL) terminal window from Sublime Text on right-click "
date:   2019-03-15
categories:
---
When developing on Windows with Sublime Text, it can be useful to run linux applications on the Ubuntu installation available on Windows Subsystem for Linux.  This can be awkward to navigate when working in large projects, so here's how you can add a right-click option to open a terminal window in the context of any folder in the sidebar:

1. Install the Sublime Terminal package.  Press `Ctrl+Shift+P` choose `Package Control: Install Package`, and then search for and install the `Terminal` package.
2. Open the configuration file at: **Preferences > Package Settings > Terminal > Settings – User**
3. Enter in the following configuration:
```
{
	"terminal": "C:\\Windows\\System32\\bash.exe"
}
```
4.  Now when you right click in your sidebar, you should see the option: `Open Terminal Here...`