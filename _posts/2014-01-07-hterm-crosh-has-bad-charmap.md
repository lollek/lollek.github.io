---
layout: post
title: Crosh / HTerm send bad charmap
---

Just posting this in case anyone else has the same issue. I know that I spent
quite some time googling for this answer without finding anything..

Crosh/hterm in ChromeOS has a messed up locale charmap. If you type/paste
non-ascii, it gets encoded as ISO-8859-1 even though everything else is UTF-8.
I dived into the source a bit today to see if I could find something, and this
seems to be the culprit:

{% highlight js %}
term_.vt.encodeUTF8 = function (str) {
    return lib.encodeUTF8(str);
}
{% endhighlight %}

This seems to change my UTF-8 to latin1 (why?), so this is my workaround for
every time I open crosh

{% highlight js %}
1. Open Javascript Console (Ctrl-Shift-J)
2. Paste "term_.vt.encodeUTF8 = function(str) {return str;}" and press enter
{% endhighlight %}

Also opened a ticket here: https://code.google.com/p/chromium/issues/detail?id=331519
