---
layout: post
title: Jekyll Now Supports Auto-Regeneration 
code:
- type: bash
  count: 0

- type: bash
  count: 1


---
Jekyll has just added a new feature which is Auto- Regeneration. This update is
really neat and saves web developers a lot of time. Before the update, Jekyll
has given developers the option to launch their website on the local server
`localhost:4000` by using the command:

{% highlight bash linenos %}

jekyll serve

{% endhighlight %}

However, anytime a change is made you would have to rerun the command to
regenerate the site.Now Jekyll has a neat feature known as auto-regeneration. Basically, it will
watch for any changes in the directory of your website and will automatically
regenerate anytime a change is made. You just need to add `--watch` as so:

{% highlight bash linenos %}

jekyll serve --watch

{% endhighlight %}

