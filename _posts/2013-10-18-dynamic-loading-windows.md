---
title: How to do dynamic loading on Windows
date: 2013-10-18T09:23:12+00:00
author: chs
layout: post
permalink: /dynamic-loading-windows/
categories:
  - Code
---
So, after hitting DerbyCon a few weeks ago and going to some of the malware talks, I was reminded of my Windows programming days and decided to shake the rust off and do some examples. Today&#8217;s example is of how to load a dll dynamically in Windows.

The following is an example function that I created and put in a DLL called DynamicLoadedDLL.dll. The actual creation of the project and how to build is beyond the scope of this post. But, basically I created an empty solution and added a Win32 Project called DynamicLoadedDLL and specified the type DLL.

The code for the DLL in DynamicLoadedDLL.cpp is-

<pre lang="C" line="1">#include "stdafx.h"

extern "C"
{
	__declspec( dllexport ) int AddTwo(int a, int b)
	{
		return a + b;
	}
}
</pre>

Basically what that does is define a function called AddTwo that takes two integers and returns their sum. __declspec( dllexport ) tells the compiler to add the function to the Export Address Table so that it is exposed as a function in the DLL. The &#8220;extern &#8220;C&#8221;&#8221; block on line 3 tells the compiler to expose it without the C++ name mangling. This makes it callable by many different languages. If you want to see the difference of having it vs. not, first open a developer command prompt and go to the output directory. Run the command &#8220;dumpbin /exports DynamicLoadedDLL.dll&#8221; and see what it shows. Then remove the &#8220;extern &#8220;C&#8221;&#8221; block, build, and repeat.

Then I created a Win32 Console app in the same solution called DynamicLoader and it&#8217;s code is-

<pre lang="C" line="1">#include "stdafx.h"

typedef int (*AddTwo)(int, int);

int _tmain(int argc, _TCHAR* argv[])
{
	HMODULE hLib = LoadLibrary(TEXT("DynamicLoadedDLL.dll"));
	printf("\r\nDynamicLoadedDLL.dll loaded into memory at 0x%x\r\n", hLib);

	AddTwo AT = (AddTwo) GetProcAddress(hLib, "AddTwo");
	printf("AddTwo method loaded into memory at 0x%x\r\n", AT);
	printf("6 + 5 = %d\r\n", AT(6,5));
        CloseHandle(hLib);
	return 0;
}</pre>

Line 3 defines the function pointer. It says create a type called AddTwo that is a pointer to a function that takes 2 integers as a parameter and returns an integer. If you were statically linking to a library, you would have the header file and the lib file and wouldn&#8217;t need to do this. Line 7 is how we load the library into memory. It looks in the standard Windows search path for the library. If it can load the library the hLib parameter actually points to where the library is loaded in the memory space of the process as demonstrated by line 8. Line 10 gets the address of the &#8220;AddTwo&#8221; function from the library and creates a function called AT that points to it. Line 11 shows the address of the function in memory. Line 12 calls the function and prints the result. Line 13 closes the handle to the library. Error checking has been left out for brevity.

The output is-

<pre lang="TEXT">DynamicLoadedDLL.dll loaded into memory at 0x585c0000
AddTwo method loaded into memory at 0x585c1010
6 + 5 = 11
</pre>

If you run &#8220;dumpbin /imports&#8221; on DynamicLoader.exe you won&#8217;t see any reference to DynamicLoadedDLL.dll at all.

That&#8217;s pretty much it!
