---
layout: post
comments: true
title: Trim your Videos Fast and Easy.
code:
- type: bash
  count: 0
 
---

I wanted to trim my video without any loss of quality. It took me a surprisingly
long time to find this on Google so I thought I would put this command here so
if someone else needs to do this, they do not need to spend the time searching I
did. Also, I would most like forget as well and come back here for reference
(hello future me!).

{% highlight java linenos%}

ffmpeg -i original.webm -ss 00:00:11 -t 00:00:28 -vcodec copy -acodec copy trimed.webm

{% endhighlight %}

You can replace the `.webm` extension with `.avi`,`.mp4` or to whatever is your video file
extension.

