---
title: 'Window.postMessage part 2 - an example'
date: 2017-01-09T23:28:45+00:00
author: chs
layout: post
permalink: /window-postmessage-part-2-an-example/
categories:
  - Javascript
  - Security
---
In [Part 1](https://www.chs.us/window-postmessage-part-1/) of the series, I talked about the basics of Window.postMessage and showed some sample code.  This post will show some real code with a demo link.   This code purposefully has some security issues which will be addressed in the third and final post of the series.

The demo code found at <https://www.chs.us/pm/source.html> is &#8211;

`<iframe src="https://www.bitbucket.me/pm/target.html" style="width:100%; height:50%;"></iframe><br />
<script language="javascript"><br />
function sendMessage()<br />
{<br />
var txt = document.getElementById("msg").value;<br />
window.frames[0].postMessage(txt, "*");<br />
return false;<br />
}<br />
</script><br />
<form method="#" method="post" onsubmit="return sendMessage()"><br />
<input type="text" maxlength="128" name="msg" id="msg"><br />
<input type="submit"><br />
</form><br />
`

What it does is load <https://www.bitbucket.me/pm/target.html> in an iframe that is 50% of the height of the window and 100% of the width.  It then has a form where you can specify text and when you hit submit, it sends a message to the iframe with the text that is specified in the form.

* * *

The code for <https://www.bitbucket.me/pm/target.html> is &#8211;`<br />
<script language="javascript"><br />
window.addEventListener("message", receiveMessage, false);<br />
function receiveMessage(event)<br />
{<br />
var origin = event.origin || event.originalEvent.origin; // For Chrome, the origin property is in the event.originalEvent object.<br />
var cont = document.getElementById('container');<br />
cont.innerHTML = cont.innerHTML + event.data + "<br/>";<br />
}<br />
</script><br />
<div id="container" name="container" style="width:100%; height:100%;"></div><br />
`

It first creates registers a function(receiveMessage) to handle the &#8220;message&#8221; event and creates a div.  When a message is received it, it  appends the text to the div.
