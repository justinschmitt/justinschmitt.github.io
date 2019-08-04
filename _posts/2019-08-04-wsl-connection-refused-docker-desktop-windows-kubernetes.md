---
layout: post
title:  "Fixing connection refused error with WSL and Docker Desktop for Windows"
date:   2019-08-04
categories:
---
When installing (or reinstalling) Docker Desktop with Kubernetes on a Windows system, don't forget that Docker Desktop installs the relevant kubeconfig file only on your windows host, and *not* in the directory required for `kubectl` access within your WSL (Windows Subsystem for Linux) installation!

If you receive the error `The connection to the server localhost:6445 was refused - did you specify the right host or port?`, this may be your issue.

You can resolve the issue by copying the kubeconfig file to your WSL installation by running:

    cp /mnt/c/Users/<YourUsername>/.kube/config ~/.kube/config