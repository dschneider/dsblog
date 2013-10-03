---
layout: post
title: XBMC - DVD menu not displayed / Black screen when playing DVDs
tags: 
---
I experienced that whenever I inserted a DVD on my Ubuntu-driven HTPC and
attempt to play it using XBMC I had a black screen without any visible menu or
sound. When pressing the stop button, my computer froze completely. This bug
is filed here:

[https://bugs.launchpad.net/ubuntu/+source/xbmc/+bug/999254](https://bugs.laun
chpad.net/ubuntu/+source/xbmc/+bug/999254)

After searching and testing around a while I found an interesting option in
the XBMC Video Settings under the DVDs tab. There is an option called "Attempt
to skip introduction before DVD menu". I switched it on and suddenly
everything works. No more black screen and no more freezing when pressing the
stop button!

My second guess would have been the solution described here:

[http://harikrish.wordpress.com/2010/01/14/how-to-play-dvd-using-
xbmc/](http://harikrish.wordpress.com/2010/01/14/how-to-play-dvd-using-xbmc/)

Apparently disabling auto mount solves this problem as well. So if the first
solution doesn't work for you try the latter one.

