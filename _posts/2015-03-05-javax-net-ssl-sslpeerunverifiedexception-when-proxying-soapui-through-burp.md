---
title: javax.net.ssl.SSLPeerUnverifiedException when proxying SoapUI through Burp
date: 2015-03-05T15:44:35+00:00
author: chs
layout: post
permalink: /javax-net-ssl-sslpeerunverifiedexception-when-proxying-soapui-through-burp/
categories:
  - Security
---
Ever try to proxy SoapUI through Burp when accessing an endpoint over ssl and get this error?

<img title="SSL Error in SOAPUI.png" src="/wp-content/uploads/2015/03/SSL-Error-in-SOAPUI.png" alt="SSL Error in SOAPUI" width="222" height="11" border="0" />

Here’s how to fix-

First, in your SoapUI script(s), change the protocol of all of the endpoints from https to http.

Then go to the Proxy Listeners section in Burp and edit your current proxy.

<img title="Proxy Listeners Settings in Burp.png" src="/wp-content/uploads/2015/03/Proxy-Listeners-Settings-in-Burp.png" alt="Proxy Listeners Settings in Burp" width="595" height="140" border="0" />

Then go to the Request Handling Settings and select Force use of SSL

<img title="Request Handling Settings in Burp.png" src="/wp-content/uploads/2015/03/Request-Handling-Settings-in-Burp.png" alt="Request Handling Settings in Burp" width="350" height="203" border="0" />

This basically achieves several things.  It removes the untrusted cert error that SoapUI gives because the Burp SSL proxy cert doesn’t match the endpoint, isn&#8217;t a trusted cert, or isn’t in the Java keystore. It also allows you to view all of the traffic in Burp easily. Just remember that the Force use of SSL setting in Burp for the proxy forces all traffic through through the proxy to go over ssl, so be sure to change it back when finished.
