---
layout: post
title:  "Install Windows 11 without logging in to a Microsoft account"
date:   2022-11-29
categories:
---
Do you want to install Windows 11, but you don't want to log in with a Microsoft account?   The option to install with a local account is hidden by default, but here's how you can get it back:

1.  Install normally until you get to the prompt to log in with a Microsoft account.
2.  Press `Shift + F10` to open the Command Prompt.
3.  Enter the command `oobe\bypassnro`
4.  The installer will restart.
5.  When you get back to the "Let's Connect you to a network" page, click `I don't have internet`
6.  On the next page, select `Continue with limited setup`

You will now be able to configure a local account!