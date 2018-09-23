---
title: Injection on Windows
date: 2016-08-29T00:47:02+00:00
author: chs
layout: post
permalink: /injection-on-windows/
categories:
  - Security
---
So, I&#8217;ve been playing around a bit with DLL injection on Windows. The basic process is-

  * Identify the process
  * Open the target process
  * Create a buffer in the target process that&#8217;s large enough to hold the path to the DLL to inject
  * Write the path to the DLL to the buffer
  * Create a remote thread in the target process using LoadLibrary as the thread function and the buffer created as the parameter

That&#8217;s it in a nutshell. Will show in-depth soon!
