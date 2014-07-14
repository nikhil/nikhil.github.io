---
layout: post
title:  Adding your custom font to i3status
code:
- type: css 
  count: 0
- type: bash
  count: 1
- type: file
  file: config
  count: 2
- type: file
  file: i3status.conf
  count: 3
---
Now that I have created my custom icon font. I would like to add this font to
my i3status. This will be a challenge since before I was able to use a `css
selector` to add my icon. I cannot use css selectors in my i3status config.
However, if you look at the `css` file generated with your font located in:
<br>
<br>

<blockquote>
~/svgimages/app/assets/stylesheets/IconicIcons.css
</blockquote>

{% highlight css linenos %}

.icon-arch:before 
{ 
content: "\f100"; 
}

{% endhighlight %}

You can see
the each icon has its own unique `unicode` sequence.
###Add the font to your font cache
Now that you have the `ttf` file for your font. You can now add it to you font
cache.
{% highlight java linenos %}
mkdir ~/.local/share/fonts
cp *.ttf ~/.local/share/fonts
fc-cache -fv
{% endhighlight %}
Now you should add the custom font into the i3status config located in:
<blockquote>
~/.i3/config
</blockquote>
And edit the font line to contain your font.
{% highlight css linenos %}
font pango:Source Sans Pro, IconicIcons 11
{% endhighlight %}
After that you can then add the icon in the status messages in your
`i3status.conf` located in:
<blockquote>
~/.i3/i3status.conf
</blockquote>
{% highlight java linenos %}
tztime local {
        format = " %a %m-%d-%y %l:%M:%S %p"
}

load {
        format = " %1min"
}

disk "/" {
	format = "/: %free"
}


{% endhighlight %}
I decided to put the arch logo in front of the `/` disk. To insert unicode into
the file in `vim` you just need to:
<br>
<br>
`ctrl + v` + `u` + `unicodesequence` <br>
so for the Arch icon it would be:<br>
`ctrl + v` + `u` + `f100`<br>
After that, all you need to do is refresh i3 by:<br>
`ctrl + shift + r` <br>
Now, sit back and admire your icons on your i3status bar!
Here is picture of my i3 status. To view the full screen image click <a href = "/images/i3status.png"> here.</a>
<br>
<br>
<img src="/images/i3status.png" alt="i3status"/>
<br>
<br>
Here is a close up picture of the <i class="icon-Arch"></i> icon on my i3 status. To view the full screen image click <a href = "/images/i3statusclose.png"> here.</a>
<br>
<br>
<img src="/images/i3statusclose.png" alt="i3status"/>





