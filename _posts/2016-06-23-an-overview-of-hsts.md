---
title: An Overview of HSTS
date: 2016-06-23T16:02:50+00:00
author: chs
layout: post
permalink: /an-overview-of-hsts/
categories:
  - Security
---
## What is HSTS?

HSTS stands for HTTP Strict Transport Security.  It&#8217;s a web security policy that allows a web server to inform a web browser that it should only be accessed over HTTPS and never HTTP.  It also helps prevent things like downgrade attacks (switching from HTTPS to HTTP).

## How do you set it up?

HSTS is a response header that is set by the web server and sent back to the web browser.   In the HTTP section of your web configuration, you need to configure it to redirect the client to access via HTTPS.  In the HTTPS section of your web configuration is where you set the HSTS header.   From that point on (until the header times out), your browser will only access your site via HTTPS automatically.  Even if you try to access via HTTP, it will enforce you accessing via HTTPS.  This way of doing it allows a client browser to access via HTTP once.

HSTS can also be enforced by being in the browsers&#8217; HSTS preload list.

## Web server setup for chs.us

This is what the relevant part of the HTTP VirtualHost would look like-

<pre lang="txt" line="1">#Redirect to HTTPS if requested via HTTP
&lt;IfModule mod_rewrite.c>;
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI}
&lt;/IfModule>;
</pre>

This is what the relevant part of the HTTPS VirtualHost would look like-

<pre lang="txt" line="1">#Set HSTS
Header always set Strict-Transport-Security "max-age=108000; preload"
</pre>

This is what a response from a request to chs.us over HTTPS looks like-

<pre lang="txt" line="1">HTTP/1.1 200 OK
Date: Thu, 23 Jun 2016 13:32:23 GMT
Server: localhost
Strict-Transport-Security: max-age=108000; preload
Last-Modified: Thu, 23 Jun 2016 13:32:24 GMT
Pragma: public
Cache-Control: max-age=7200, public
Vary: Accept-Encoding
X-XSS-Protection: 1; mode=block
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
X-Powered-By: chs.us
Content-Length: 64416
Connection: close
Content-Type: text/html; charset=UTF-8</pre>

## What does the header look like?

Strict-Transport-Security: max-age=<seconds>; includeSubdomains; preload

The parameters are-

  * max-age is the number of seconds you want the browser to cache the header.  Keep in mind that every time time you get a response over HTTPS, the max-age in the client browser is reset
  * includeSubdomains informs the browser to also enforce HTTP on subdomains by default
  * preload is a tag that announces the fact that your site can be added to the default preload list that the browsers have built-in.  If your site is in this list, any time a browser access the site it will be over HTTPS.  You can add your site to the list by going to <http://hstspreload.appspot.com>.  Most browsers use this shared list, so it will be included in future browser updates.   The reason the preload tag exists is so that not just anyone can add a site to the preload list.   If they could, they would be able to deny service to sites that don&#8217;t have HTTPS, but are in the pre-load list.
