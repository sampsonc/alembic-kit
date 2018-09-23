---
title: What is X-Frame-Options?
date: 2013-11-15T10:52:21+00:00
author: chs
layout: post
permalink: /x-frame-options/
categories:
  - Security
---
X-Frame-Options is an HTTP response header that serves as a directive to a browser regarding if or how the current response would like itself to be framed. Framing and window positioning/layering tricks are used quite often in an attack called clickjacking in which a user is tricked into inadvertently clicking on something unintended. For more information see <https://www.owasp.org/index.php/Clickjacking>.

There are 3 options for X-Frame-Options-

  * DENY. This tells the browser that the page should never be framed.
  * SAMEORIGIN. This tells the browser that the page can be framed within a page that is from the same origin.
  * ALLOW-FROM uri. This tells the browser that the page can be framed on the specified origin.

This site, for example, is set to SAMEORIGIN. So in effect, a page on this origin could be framed by another page in this origin.

This is supported by most newer browsers. Should a site attempt to frame a site and it violates the site policy, it will either display about:blank in the frame or perhaps an error. If a browser doesn&#8217;t support the header, it will simply ignore it.

Another mechanism you might see as a clickjacking mitigation that tries to achieve the same goal is &#8220;frame-busting JavaScript&#8221;. This is JavaScript code that detects if the page is the top-level window (not framed). If it detects that it isn&#8217;t, it makes itself the top-level window. Since this is code that is run client-side, it does have several weaknesses such as JavaScript not being enabled, the response being modified to remove it, etc.
