---
title: The Witchcraft Compiler Collection by @endrazine
date: 2016-08-09T21:30:50+00:00
author: chs
layout: post
permalink: /witchcraft-compiler-collection-endrazine/
categories:
  - Open Source
  - Security
---
In case you missed Defcon 24 or were there and happened to miss this talk, this is some amazing stuff. It&#8217;s called the Witchcraft Compiler Collection (WCC) by my co-worker and friend, [Jonathan Brossard](https://twitter.com/endrazine).

Some things you can do with WCC:

  * Transforming ET_EXEC ELF executables into shared libraries (id est: transforming an executable into a shared library !) Demoed this by patching proftpd into a shared library and then calling functions in it from C.
  * Unlinking ELF binaries into relocatable object files, then relink them back using gcc and verify they still work !
  * Running OpenBSD binaries natively on linux by relinking it. 0 patching required !
  * Using ET_DYN executables as shared libraries (Used /usr/sbin/apache2 as a shared library ! Called internal functions from C code)
  * Prototyping exploits from symbolic execution partial traces (did a live exploit from an old version of Samba)
  * In memory JIT translation from ARM to Intel x86_64 + debugging : did a demo on running a ARM library natively on amd64 linux with inprocess JIT binary translation.</i> </ul>
    Abstract of the talk : <https://defcon.org/html/defcon-24/dc-24-speakers.html#Brossard>

    The slides are available here : [https://github.com/endrazine/wcc/blob/master/doc/presentations/Jonathan\_Brossard\_Witchract\_Compiler\_Collection\_Defcon24\_2016.pdf](https://github.com/endrazine/wcc/blob/master/doc/presentations/Jonathan_Brossard_Witchract_Compiler_Collection_Defcon24_2016.pdf)

    The codebase is available here : <https://github.com/endrazine/wcc> under MIT License (proper open source).

    The code for all the demos is available here : [https://github.com/endrazine/wcc/tree/master/doc/presentations/demos\_defcon24\_2016](https://github.com/endrazine/wcc/tree/master/doc/presentations/demos_defcon24_2016)

    Check it out!
