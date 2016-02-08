---
layout: post
comments: true
title: SwitchBeat

code:
- type: packages
  count: 0

---
<img src="../images/SwitchBeat.png" alt="SwitchBeatHome">
<br>&nbsp;<br>
<img src="../images/projects/SwitchBeat.png" alt="SwitchBeatAnalysis">

I decided to spend my winter break programming something I was interested in. I
really enjoy listening to `mashups`, which are two or more songs that are combined into one.
I often enjoy listening to mashups in <a
href="http://www.reddit.com/r/mashups/">r/mashups</a> on Reddit. I thought it
would be really cool to make a program that dynamically mashups any two songs
that you provide. I have spent most of my winter break working on this. It is by far the
largest program I have ever written.


## Check it out

<a href="http://kumarcode.com/switchbeat.html" alt="kumarcode.com/switchbeat">kumarcode.com/switchbeat.html</a>

## Examples

<a href="http://kumarcode.com/switchbeatexamples.html" alt="kumarcode.com/switchbeatexamples">kumarcode.com/switchbeatexamples.html</a>

## Uses

{% highlight java linenos %}
Raphaël.js
Chart.js
{% endhighlight %}

## Description

Takes any two songs that you supply and creates a mashup between them. 

### How it works

The program was inspired by the infinite jukebox. Music Analysis is done on
the Echonest servers. SwitchBeat uses this analysis to compare the beats of
both songs. Using this analysis it creates a normally distributed beat distance
dataset. Then using the parameters you specify it generates connections
between both songs.

### How to use it

#### 1. Select the parameters 

`%` for <b>music connections</b>: Increasing this will increase the number of
connections created. I have set the default to `5%`.<br>
`%` for <b>connection jump</b>: Increasing this will increase the chance that
SwitchBeat will jump on this connection. I have set the default to `30%`.<br>
`#` for <b>maximum connections per beat</b>: Increasing this will increase max
number of connections a beat can have. I have set the default to `4`.<br>
`on/off` for <b> Infinite Mode</b>: If it is on, when the program reaches the end
of one song it will jump to the start of another. I have set the default to
`on`.<br>

#### 2. Upload songs

You will be asked to upload your songs one by one. 
Please upload a `.mp3`, `.flacs`, `.m4a`, or `.opus` will not be
accepted. Try to upload in good quality `320 kbps` or `V0` is fine

#### 3. Wait for the analysis to finish

I have created a neat loading animation using `chart.js`

#### 4. Play the Mashup!

Have fun mashing up your favorite songs.



