---
title: 'Window.postMessage part 1 - the basics'
date: 2016-12-26T23:08:43+00:00
author: chs
layout: post
permalink: /window-postmessage-part-1/
categories:
  - Javascript
  - Security
---
**What is Window.postMessage?**

Window.postMessage is a way to safely communicate cross-origin between windows. Normally, pages are only allowed to interact with each other if they share the same origin(protocol+host+port matching). postMessage allows the a developer to get around that.

**Syntax**

<pre lang="javascript">targetWindow.postMessage(message, targetOrigin, [transfer]);
</pre>

The components are-

  * **targetWindow** &#8211; a reference to another window that you wish to send a message to. You can obtain a reference by a) the object returned by window.open, b) the contentWindow of an iframe, and c) using the numeric index on the Window.frames object
  * **message** &#8211; data sent to the window
  * **targetOrigin** &#8211; specify what the origin of targetWindow must be in order for the event to be dispatched. Either &#8216;\*&#8217; or the full origin. This is necessary because the origin of the window may have changed. It is advised not to specify &#8216;\*&#8217;, otherwise you may be sending data to an unintended origin.
  * **transfer (optional)** &#8211; objects that are transferred with the message. After transferring, they are no longer accessible by the sender

**What does it look like in practice?**

The code in the target window (www.mytarget.com) would look something like this. This script lives on the default page for www.mytarget.com.

<pre lang="javascript">window.addEventListener("message", receiveMessage, false);
function receiveMessage(event)
{
  var origin = event.origin || event.originalEvent.origin; // For Chrome,
  if (origin !== "http://www.mysender.com")
    return;
  else
    //dosomething
}
</pre>

The above code is based off of <https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage>

This is what the sending code would look like (on http://www.mysender.com)

<pre lang="javascript">var wnd = window.open("http://www.mytarget.com);
wnd.postMessage("Test Message", "http://www.mytarget.com");
</pre>

That&#8217;s basically what it looks like and how you use Window.postMessage to send messages cross-origin. This gets around the same origin policy which restricts how windows can interact with each based on having the same protocol, host, and port. Part 2 of the series will continue the discussion with the security implications of Window.postMessage and how using it improperly can lead to unintended security vulnerabilities in a page.
