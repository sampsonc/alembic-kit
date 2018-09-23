---
title: 'Fun with C++ - Placement New'
date: 2013-12-04T12:12:41+00:00
author: chs
layout: post
permalink: /placement-new/
categories:
  - Code
---
So, for some reason in the last few days I&#8217;ve been thinking about the old days of when I was a developer and really into C/C++. One of the things that I found interesting was placement new. In a nutshell it&#8217;s a way that you can instantiate a new object, but specify where in memory it will go. There are some interesting cases where it&#8217;s necessary such as cases where you might try to optimize by pre-allocating a bunch of memory and then re-using it. Kind of managing your own memory space. Anyway, this example shows how it&#8217;s done.

<pre lang="C" line="1">#include "stdafx.h"


class Foo
{
public:
  Foo() { strncpy(message, "Hello, World", 12);}
  void * operator new (size_t, void * p) throw() { return p ; }
  void operator delete (void *, void *) throw() { }
  void printMessage() { printf("message is - %s\n", message); }
private:
  char message[13];
};

int _tmain(int argc, _TCHAR* argv[])
{
  char buf[sizeof(Foo)];
  memset(buf, 0, sizeof(buf));
  Foo *f = new((void *)buf) Foo();
  printf("address of buf - %x\n", buf);
  printf("address of f - %x\n", f);
  printf("contents of buf - %s\n", buf);
  f->printMessage();
  buf[0] = 'h';
  f->printMessage();
  f->~Foo();
  return 0;
}
</pre>

Line 8 defines the constructor that placement new uses.

Line 9 defines the destructor.

Line 17 creates a buffer that is the size of a Foo object.

Line 19 instantiates a Foo object, but uses the buffer as the address of where the object will be instantiated.

Lines 20 and 21 print the addresses of the buffer and of f (the new object). They are the same.

Line 22 prints the contents of the buf, which is &#8220;Hello, World&#8221;. This is the value of the message variable in the object.

Line 23 prints the message.

Line 24 changes the first character in buf to be &#8216;h&#8217;. This should change it within f as well.

Line 25 shows that it changed.

Line 26 explicitly calls the destructor. That needs to be done when using placement new.

That&#8217;s about it! The example created a buffer, created an object in that buffer, changed the buffer, and showed that the object changed. Fun stuff!

Running it on my machine show-

<pre>address of buf - 31fbe0
address of f - 31fbe0
contents of buf - Hello, World
message is - Hello, World
message is - hello, World
</pre>
