---
title: Auth Header Updater
date: 2013-09-14T19:38:43+00:00
author: chs
layout: page
permalink: /authheaderupdater/
---
AuthHeaderUpdater is a Burp extension that allows you to specify the Authentication: Bearer header token value that is used during scanning.

### Screenshot
![Auth Header Updater](https://www.chs.us/images/serve.php?img=authheaderupdater&src=chs.us)

### Installing

Go to Extender Tab -> Add.  Specify the jar file.  Click Next and then Close.  Notice the new "Auth Header Updater Tab"

### Usage

Specify the new token value in the "Auth Bearer Token" text box and click "Enabled".  

It will then replace 

```
Authentication: Bearer <token>
```

with 

```
Authentication: Bearer <value from the extension>
```

while doing a scan.  

Uncheck "Enabled" to disable the extension.

## Authors

* **[Carl Sampson](https://www.chs.us)** -  [@chs](https://twitter.com/chs)

## License

This project is licensed under the MIT License - see the [LICENSE.md](https://github.com/sampsonc/AuthHeaderUpdater/blob/master/LICENSE.md) file for details

## Link

The source and binary are on [Github](https://github.com/sampsonc/AuthHeaderUpdater).  The binary is in the dist folder.


