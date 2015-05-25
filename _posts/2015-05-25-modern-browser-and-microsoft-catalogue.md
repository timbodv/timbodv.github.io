---
layout: post
title: Using a Modern Browser with the Microsoft Catalogue
---

The [Microsoft Update Catalogue](http://catalog.update.microsoft.com/v7/site/Home.aspx) website doesn't appear to work with anything but Internet Explorer, and even then I've had issues trying to download drivers or updates. Typically I'm also in a rush so don't have the time to spin up a Windows 7 box with IE8.

As a workaround you can use Internet Explorer (tested with v11) to search for the driver or update you require, and then use another modern browser (tested with Chrome v42) to download the file without having to add it to the download basket.

1. Search for the driver or update using Internet Explorer
2. When you've found the package you want, click the __Title__ to open a seperate window with further details
3. Under the __Package Details__ tab, copy the guid from the __Supported Update IDs__ panel
4. Append the guid to [http://catalog.update.microsoft.com/v7/site/ScopedViewRedirect.aspx?updateid=]() and open the new URL in a non-IE browser (if you continue to use IE you may still have download issues)
5. Click the __Download Now__ button to be redirected to another window with a direct URL to download the package CAB file
