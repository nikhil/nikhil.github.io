---
layout: post
comments: true
title: Making your own custom icon font. 
code:
- type: bash 
  count: 0
- type: packages
  count: 1
- type: bash
  count: 2 
- type: bash
  count: 3 
- type: html 
  count: 4 
  
---

I have recently decided to spice up my i3 status by adding icons to all of my info display sections. However, I also wanted to add an Arch icon in the front. This led me to an issue, as there are no icon fonts that I could find that have the arch icon included. So I decided to make my own icon font.

###Getting the Arch icon svg
In order to create a custom icon you need to first have an `svg` image of
picture.
<blockquote>
SVG: an XML-based vector image format
</blockquote>
The Arch svg that I am looking for can be downloaded from the `Arch User
Repository`. If you are on Arch, you just need to run:
{% highlight bash linenos %}

pacaur -S archlinux-artwork

{% endhighlight %}
The Arch icons can now be found in:
<blockquote>
/usr/share/archlinux/icons
</blockquote>
You now have two choices for icons:
<br>
<img src="/images/archbody.png" class="Arch1" alt="Arch logo"/>
<img src="/images/archtango.png" class="Arch2" alt="Arch logo"/>
<span class="Arch1t"> Crystal</span>
<span class="Arch2t"> Tango</span>
<br>
<br>
It is based on your preference but I choose the Crystal icon.

###Generating the font
Now that you have the svg files, you can create the font with the program
`fontcustom`.

####Packages needed
{% highlight ruby linenos %}
fontforge
ttfautohint
Ruby1.9.x or greater
{% endhighlight %}
Now install fontcustom:
{% highlight java linenos %}

wget http://people.mozilla.com/~jkew/woff/woff-code-latest.zip
unzip woff-code-latest.zip -d sfnt2woff && cd sfnt2woff 
&& make && sudo mv sfnt2woff /usr/local/bin/
gem install fontcustom

{% endhighlight %}

Now generate the config and run fontcustom on the folder where your svg images
are stored. For me I stored my svgs in a folder called `svgImages`. You can
also add the name of your font by editing the config. I named mine
`IconicIcons` (They are iconic!)

{% highlight java linenos %}
~/svgimages/     fontcustom config  ~/svgimages
~/svgimages/     fontcustom compile ~/svgimages --debug
{% endhighlight %}

<i class="fa fa-warning"></i>  **Notice:** Fontcustom is really sensitive to the file structure of your svg file. I have encountered many errors when trying to edit the output directory structure. It is best that you leave the structure to default. Also make sure you generate the config file in the directory that you are running fontcustom. 
{: .notice}

If all goes well, this will generate `.svg`, `.ttf` and `.woff` files under: 
<blockquote>
~/svgimages/app/assets/fonts/
</blockquote>
Now, after importing the css file and storing the font files in the proper
place. You can use this font on you website by doing:
{% highlight html linenos %}
<i class="icon-arch"></i>
{% endhighlight %}
This will display the font. <i class="icon-arch"></i> Yay!





