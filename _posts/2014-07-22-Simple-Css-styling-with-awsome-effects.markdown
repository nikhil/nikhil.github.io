---
layout: post
comments: true
title: Simple Css styling with Awsome effects 
code:
- type: css
  count: 0

---
You can make you website look awsome with not much code at all. You just need a
bit of creativity, a bit of css, and alot of criticism. In this post, I will
talk about the `css transform` property. Just a few lines of code can greatly
change the appearance of your website. To get an icon to spin in the y
direction, it is simple:

{% highlight css linenos %}

.tospin
{
display: inline-block;
    -webkit-transform: rotateY(360deg);
       -moz-transform: rotateY(360deg);
        -ms-transform: rotateY(360deg);
         -o-transform: rotateY(360deg);
            transform: rotateY(360deg);

}
{% endhighlight %}


### Browser support
<p>Different browsers have different versions of the same css command.</p>
<div class= "browsercontainer">
<div class="property">
<div class="propertytxt">
   -webkit-transform: rotateY(360deg);<br>
       -moz-transform: rotateY(360deg);<br>
        -ms-transform: rotateY(360deg);<br>
         -o-transform: rotateY(360deg);<br>
</div>		 
</div>
<div class="browser">
<i class="icon-chrome"></i>&nbsp;<i class="icon-safari"></i>&nbsp;<i class =
"icon-opera"></i>
<br>
<i class="icon-firefox"></i>
<br>
<i class="icon-IE"></i>
<br>
<i class="icon-opera"></i>
</div>
</div>
In addition, to make things run smoother, especially on firfox make sure to
include the line `display: inline-block;`



