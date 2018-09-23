---
id: 231417
title: 'BinPeek - an app to determine if a #Windows executable is managed or unmanaged.'
date: 2017-12-30T19:47:44+00:00
author: chs
layout: post
guid: http://www.chs.us/?p=231417
permalink: /binpeek-app-determine-windows-executable-managed-unmanaged/
ssb_twitter_counts:
  - "0"
ssb_fbshare_counts:
  - "0"
ssb_linkedin_counts:
  - "0"
ssb_reddit_counts:
  - "0"
ssb_total_counts:
  - "0"
ssb_cache_timestamp:
  - "424221"
dsq_thread_id:
  - "6382111509"
ssb_old_counts:
  - 'a:6:{s:10:"googleplus";i:0;s:7:"twitter";i:0;s:9:"pinterest";i:0;s:7:"fbshare";i:0;s:8:"linkedin";i:0;s:6:"reddit";i:0;}'
categories:
  - Code
  - Open Source
---
BinPeek is an application that checks to see if a Windows application is managed(.NET) or unmanaged(native). It handles x86 and x84 executables. If doing it manually, you must check several values in the PE (Portable Executable) file header that differ slightly based on whether the executable is 32-bit or 64-bit. BinPeek does that work for you.

## Usage

<pre>D:\source\repos\BinPeek>binpeek BinPeek.exe
BinPeek.exe --> Unmanaged
</pre>

## Project Page on Github

<a href="https://www.github.com/sampsonc/BinPeek" target="_blank"><img src="http://www.chs.us/wp-content/uploads/2017/12/binpeek-300x197.png" alt="" width="300" height="197" class="alignnone size-medium wp-image-231460" srcset="https://www.chs.us/wp-content/uploads/2017/12/binpeek-300x197.png 300w, https://www.chs.us/wp-content/uploads/2017/12/binpeek-768x505.png 768w, https://www.chs.us/wp-content/uploads/2017/12/binpeek.png 997w" sizes="(max-width: 300px) 100vw, 300px" /></a>

## Install

Build with Visual Studio or just use the release version in the repo.

## License

MIT
