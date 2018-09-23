---
title: XML External Entity attack (XXE) in a Nutshell
date: 2015-04-03T09:15:42+00:00
author: chs
layout: post
permalink: /xml-external-entity-attack-xxe-in-a-nutshell/
categories:
  - Security
---
The XXE attack has been around for a few years, but hasn&#8217;t gotten much attention until the last couple of years with some high-profile cases in Facebook and PayPal.

So, what is the XML External Entity attack? XXE is an abbreviation for XML External Entity. It is a part of the XML spec that allows a document to have entities that resolve to someplace external (not within the same document).

Some basic examples demonstrate the concept describe it best. For example, let&#8217;s say that we have a web app that takes as input an xml file and displays it in a table.

**Example 1**

Here&#8217;s a sample input file-

<pre lang="xml" line="1"><?xml version="1.0" encoding="utf-8"?>
  &lt;contacts>
    &lt;contact>
      &lt;login>bobw&lt;/login>
      &lt;name>Bob Walker&lt;/name>
      &lt;email>bob@bob.com&lt;/email>
    &lt;/contact>
    &lt;contact>
      &lt;login>ajones&lt;/login>
      &lt;name>Alice Jones&lt;/name>
      &lt;email>alice@alice.com&lt;/email>
    &lt;/contact>
&lt;/contacts>
</pre>

This is processed and displays the following-

| login  | name        | email           |
| ------ | ----------- | --------------- |
| bobw   | Bob Walker  | bob@bob.com     |
| ajones | Alice Jones | alice@alice.com |

</body>

Pretty Straightforward, right?

* * *

**Example 2**</p>

Now, let&#8217;s take the same example and add an entity-

<pre lang="xml" line="1"><?xml version="1.0" encoding="utf-8"?>

]>
  &lt;contacts>
    &lt;contact>
      &lt;login>&foo;&lt;/login>
      &lt;name>Bob Walker&lt;/name>
      &lt;email>bob@bob.com&lt;/email>
    &lt;/contact>
    &lt;contact>
      &lt;login>ajones&lt;/login>
      &lt;name>Alice Jones&lt;/name>
      &lt;email>alice@alice.com&lt;/email>
    &lt;/contact>
&lt;/contacts>
</pre>

This processes and displays-

| login  | name        | email           |
| ------ | ----------- | --------------- |
| Foo    | Bob Walker  | bob@bob.com     |
| ajones | Alice Jones | alice@alice.com |

What happened? On line 3 of the xml file we created an entity called foo which is the string, &#8220;Foo&#8221;. We then use that entity, &foo, in place of Bob&#8217;s username on line 7. While processing the document the parser substituted &#8220;Foo&#8221; when it saw &foo;.

* * *

**Example 3**</p>

Now let&#8217;s do something really interesting. Consider the following-

<pre lang="xml" line="1"><?xml version="1.0" encoding="utf-8"?>

]>
  &lt;contacts>
    &lt;contact>
      &lt;login>&foo;&lt;/login>
      &lt;name>Bob Walker&lt;/name>
      &lt;email>bob@bob.com&lt;/email>
    &lt;/contact>
    &lt;contact>
      &lt;login>ajones&lt;/login>
      &lt;name>Alice Jones&lt;/name>
      &lt;email>alice@alice.com&lt;/email>
    &lt;/contact>
&lt;/contacts>
</pre>

This processes and displays-

| login                                      | name        | email           |
| ------------------------------------------ | ----------- | --------------- |
| root:x:0:0:root:/root:/bin/bash <redacted> | Bob Walker  | bob@bob.com     |
| ajones                                     | Alice Jones | alice@alice.com |

What did it do? On line 3, the keyword SYSTEM means that this entity reference is external to the document. In this case, the external entity references /etc/passwd on the system that is processing the xml. This causes the contents of /etc/passwd to be pulled into the document and then displayed.

* * *

**Example 4**</p>

Up to this point, the attacks have been against the server. How can we attack the user?

Consider this-

<pre lang="xml" line="1"><?xml version="1.0" encoding="utf-8"?>

]>
  &lt;contacts>
    &lt;contact>
      &lt;login>&foo;&lt;/login>
      &lt;name>Bob Walker&lt;/name>
      &lt;email>bob@bob.com&lt;/email>
    &lt;/contact>
    &lt;contact>
      &lt;login>ajones&lt;/login>
      &lt;name>Alice Jones&lt;/name>
      &lt;email>alice@alice.com&lt;/email>
    &lt;/contact>
&lt;/contacts>
</pre>

What do you think the external entity reference does here? It returns <script>alert(&#8216;xss&#8217;)</script>. When the table displays that script is executed in the browser. (I&#8217;m not displaying the results like in previous examples because it would execute while you are reading this and it&#8217;s just an example showing that it&#8217;s vulnerable.).

I hope these examples give you a basic understanding of what the XXE vulnerability is. I&#8217;ll likely do a follow-up post with more advanced examples soon and how to mitigate it soon.
