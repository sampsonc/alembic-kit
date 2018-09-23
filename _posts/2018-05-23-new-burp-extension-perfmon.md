---
title: 'New Burp extension - perfmon'
date: 2018-05-23T09:18:15+00:00
author: chs
layout: post
permalink: /new-burp-extension-perfmon/
categories:
  - Burp
  - Security
---
Just whipped together a new <a href="https://portswigger.net/burp" rel="noopener" target="_blank">Burp</a> extension called <a href="https://github.com/sampsonc/Perfmon" rel="noopener" target="_blank">perfmon</a> (not to be confused with the Windows tool of the same name). I was really interested in the the resource usage of Burp while doing certain activities.

It adds a new tab to Burp and samples every 5 seconds-

  * Current and max number of threads in use
  * Current and max memory used
  * Current and max memory allocated
  * Ticker to set how often the stats update. 1 &#8211; 5 seconds.

<img src="https://www.chs.us/wp-content/uploads/2018/05/perfmon1-300x120.jpg" alt="Burp Screenshot" width="300" height="120" class="alignnone size-medium wp-image-231532" srcset="https://www.chs.us/wp-content/uploads/2018/05/perfmon1-300x120.jpg 300w, https://www.chs.us/wp-content/uploads/2018/05/perfmon1.jpg 468w" sizes="(max-width: 300px) 100vw, 300px" />

I plan to add a few more things and clean up the UI a bit, but it was an interesting exercise.

The source and plugin are on <a href="https://github.com/sampsonc/Perfmon" rel="noopener" target="_blank">github</a>.
