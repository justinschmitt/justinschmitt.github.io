---
layout: post
title:  "Configure VSCode for M1 Macs to launch integrated terminal in Rosetta 2 (x86-64 mode)"
date:   2021-05-12
categories:
---
Looking for a way to quickly open a terminal under Rosetta 2 on your M1 Mac?  If you're getting tired of repeatedly entering `arch -x86_64 /bin/zsh` every time you need to run Rosetta 2 in the terminal, try this in VSCode:

1.  Open VSCode's `settings.json` by pressing `Command + Shift + P`, and then entering `Preferences: Open Settings (JSON)`
2.  Append the following to `"terminal.integrated.profiles.osx"`:
    ```
    "terminal.integrated.profiles.osx": {
        "zsh (x86-64)": {
            "path": "arch",
            "args": ["-x86_64", "/bin/zsh"]
        }
    ```

You can also, of course, replace `zsh` with your preferred shell.