---
layout: post
title: Developing Random Math Art
comments: true
code:
- type: html 
  count: 0

- type: javascript
  count: 1

- type: javascript
  count: 2

- type: javascript
  count: 3

- type: javascript
  count: 4

- type: html
  count: 5
 
---

You may have noticed the graph on the side. This is created dynamically by Javascript using random numbers and basic trig alterations of sine graphs. After a lot of googling, I was finally able to find a method. You first need to create a `canvas` object in your html

{% highlight html linenos %}

<canvas class = "sine" id="canvas" width=160 height=635></canvas>

{% endhighlight %}

Then create a canvas object in javascript pointing to that canvas.

{% highlight javascript linenos %}

var canvas=document.getElementById("canvas");
var ctx=canvas.getContext("2d");
{% endhighlight %}


Then you have to clear the canvas for any previous lines:

{% highlight javascript linenos %}

ctx.save();

ctx.setTransform(1, 0, 0, 1, 0, 0);
ctx.clearRect(0, 0, canvas.width, canvas.height);

ctx.restore();
{% endhighlight %}

Now, draw the sine curve. There are many option you have here. I have a
randomized amplitude and frequency for every `12th` number with a max amplitude
of `75` and a max frequency of `1.5`.

{% highlight javascript linenos %}

while( x<400)
{   if(Math.floor(x)%12 ==0)
    {
          amp =  Math.floor(Math.random() * 75)+.01;
        width = Math.floor(Math.random() * 1.5)+.01;
    }    
        
    
    var y=amp*Math.sin(width*x)+ 80;
    ctx.lineTo(y,x);
     x=x+.05;
}
{% endhighlight %}


After that, you need to generate a randomized color gradient for the graph. To
do so I used `createLinearGradient`.

{% highlight javascript linenos %}

var gradient=ctx.createLinearGradient(0,0,0,300);
var color1 = '#'+(0x1000000+(Math.random())*0xffffff).toString(16).substr(1,6)
gradient.addColorStop("0",color1);
var color2 = '#'+(0x1000000+(Math.random())*0xffffff).toString(16).substr(1,6)
gradient.addColorStop(".25",color2);
var color3 = '#'+(0x1000000+(Math.random())*0xffffff).toString(16).substr(1,6)
var color4 = '#'+(0x1000000+(Math.random())*0xffffff).toString(16).substr(1,6)
gradient.addColorStop(".5",color3);
gradient.addColorStop("1.0",color4);

// Fill with gradient
ctx.strokeStyle=gradient;
ctx.lineWidth=2.4;

ctx.stroke();
{% endhighlight %}


Finally, a button to regenerate the sine.

{% highlight html linenos %}

<button onclick ="getSine()" class = "btn" id = "sinbtn"> Regenerate Sine </button>
{% endhighlight %}


