---
layout: post
comments: true
title: i3 tiling window manager 
code:
- type: bash 
  count: 0
---

## Why did I switch to a tiling window manager?
I am not going to say that a minimalistic  environment such as `i3` is better than a desktop manager such as `KDE`, `GNOME`, `MATE`, or `Xfce`, but a minimalistic environment has helped me improve my tactics. After switching to a tiling window manager, I have found myself more focused on my task at hand and more efficient with my screen usage. Before using a tiling window manager, I would often clutter up my windows on one workspace, even when I had workspace switching as an option.

## i3
I choose i3 tiling window manager. According to the i3 webpage:
<blockquote>
i3 is a tiling window manager, completely written from scratch. The target platforms are GNU/Linux and BSD operating systems, our code is Free and Open Source Software (FOSS) under the BSD license. i3 is primarily targeted at advanced users and developers. <a href="http://i3wm.org/"><i class="fa fa-desktop"></i></a></blockquote>

### Installing i3

{% highlight bash linenos %}

sudo pacman -S i3 

{% endhighlight %}

 <i class="fa fa-warning"></i> **Caution:** If you are using `startx`, make
 sure to edit your `~/.xinitrc` and add the line `exec i3`. I am using `KDM`,
 the login manager for kde to start i3.  
{: .notice}

### Neat keybindings for i3

i3 has a special `$mod` key which you would use anytime when you are using i3's
special keybindings. You can decide which key this `$mod` is. It is usually
`alt` or the `windows` key. Some of the most common used key bindings are: 

* `$mod` + `Enter`&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Open terminal
* `$mod` + `D`&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Opens the dmenu
* `$mod` + `Shift` + `Q`&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   Kills the current window
* `$mod` + `Shift` + `E`&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   Exits i3
* `$mod` + `Shift` + `R`&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   Restarts i3
* `$mod` + `#`&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;      Goes to the `#` workspace




