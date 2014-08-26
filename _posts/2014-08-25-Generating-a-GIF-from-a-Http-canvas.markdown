---
layout: post
title: Generating a .gif from a Http canvas 
comments: true
code:
- type: html 
  count: 0

- type: html
  count: 1

- type: javascript
  count: 2

- type: html
  count: 3

- type: javascript
  count: 4

- type: packages
  count: 5

- type: bash 
  count: 6

- type: html
  count: 7

- type: javascript
  count: 8

---

Today, I wanted to create a loading animation for my images on my garden page.
Since I have a lot of high quality images, I decided to use `unveil.js`. This
javascript plugin allows me to `lazy-load` my images and also present a loading image.

# Setting up the image
You need to first import unveil.js

{% highlight html linenos %}


<script src="/assets/js/jquery.unveil.js"></script>


{% endhighlight %}

Then create your image tag:

{% highlight html linenos %}

<img class="lazy" data-src="/images/gardening/lily.JPG" src="/images/loading.gif" />

{% endhighlight %}

Then run the javascript plugin:

{% highlight javascript linenos %}

$(document).ready(function() {

$(".lazy").unveil();

});
{% endhighlight %}

#Example

<img class="lazy" data-src="/images/gardening/lily.JPG" src="/images/loading.gif" />

<script>
$(document).ready(function() {

$(".lazy").unveil();


});
</script>

#Generating the animation

I decided I wanted to create my own custom loading animation. I was able to
create my own animation on the `canvas` using `javascript`.

{% highlight html linenos %}

<div id="canvas-container" style="width: 500px; height: 300px;">
    <canvas id="sineCanvas" style="width: 500px; height: 300px;" width="500" height="300"></canvas>
</div>

{% endhighlight %}

Then I created a custom animation using javascript. This is an extension of a
sample code on drawing a sine curve <a href =
"http://jsfiddle.net/umaar/fWSUk/"> here. </a>

{% highlight javascript linenos %}

(function () {

if (typeof(Humble) == 'undefined') window.Humble = {};
Humble.Trig = {};
Humble.Trig.init = init;

var unit = 100,
    canvas, context, canvas2, context2,
    height, width, xAxis, yAxis, gif,
    draw;

/**
 * Init function.
 * 
 * Initialize variables and begin the animation.
 */
function init() {
 
	canvas = document.getElementById("sineCanvas");
    
    canvas.width = 500;
    canvas.height = 300;
    context = canvas.getContext("2d");
    context.fillStyle = "#fdf6e3";
    context.fillRect(0,0,500,300);
    context.font = '18px sans-serif';
    context.strokeStyle = '#000';
    context.lineJoin = 'round';
    
    height = canvas.height;
    width = canvas.width;
    
    xAxis = Math.floor(height/2);
    yAxis = Math.floor(width/4);
    
    context.save();
    draw();
}

/**
 * Draw animation function.
 * 
 * This function draws one frame of the animation, waits 20ms, and then calls
 * itself again.
 */
draw = function () {
    
    // Clear the canvas
    //context.clearRect(0, 0, width, height);
    context.fillStyle = "#fdf6e3";
    context.fillRect(0,0,500,300);


    // Draw the axes in their own path
    context.beginPath();
    
    context.stroke();
    
    // Set styles for animated graphics
    context.save();
    context.fillStyle = 'blue';
    context.fillText("Loading...",20,150);
    context.strokeStyle = '#00f';
    context.lineWidth = 2;
   
  
    // Update the time and draw again
    draw.seconds = draw.seconds - .007;
    draw.t = draw.seconds*Math.PI;
    drawSine(draw.t);
    drawinSine(draw.t);
    setTimeout(draw, 10);
	
	
};
draw.seconds = 0;
draw.t = 0;

 function drawSine(t) {
    context.beginPath();
    context.strokeStyle = 'orange';

    var x = t;
    var y = Math.sin(x);
  

    context.moveTo(yAxis, unit*y+xAxis);
    
    // Loop to draw segments
    for (i = yAxis; i <= width; i += 10) {
        x = t+(-yAxis+i)/unit;
        
    	y = Math.sin(x);
        context.lineTo(i, unit*y+xAxis);
		context.stroke();

	 }

 context.stroke();

} 

function drawinSine(t) {
    context.beginPath();
    context.strokeStyle = 'red';   
    var x = t;
    var y = -1 * Math.sin(x);
    context.moveTo(yAxis, unit*y+xAxis);
    
    for (i = yAxis; i <= width; i += 10) {
        x = t+(-yAxis+i)/unit;
        y = -1* Math.sin(x);
        context.lineTo(i, unit*y+xAxis);
	}
    context.stroke();
}





    Humble.Trig.init()
	    
})();

{% endhighlight %}

#Example

Here is how the loading canvas animation looks:

<div id="canvas-container" style="width: 500px; height: 300px;">
    <canvas id="sineCanvas" style="width: 500px; height: 300px;" width="500" height="300"></canvas>
</div>

<script>

(function () {

if (typeof(Humble) == 'undefined') window.Humble = {};
Humble.Trig = {};
Humble.Trig.init = init;

var unit = 100,
    canvas, context, canvas2, context2,
    height, width, xAxis, yAxis, gif,
    draw;

/**
 * Init function.
 * 
 * Initialize variables and begin the animation.
 */
function init() {
 
	canvas = document.getElementById("sineCanvas");
    
    canvas.width = 500;
    canvas.height = 300;
    context = canvas.getContext("2d");
    context.fillStyle = "#fdf6e3";
    context.fillRect(0,0,500,300);
    context.font = '18px sans-serif';
    context.strokeStyle = '#000';
    context.lineJoin = 'round';
    
    height = canvas.height;
    width = canvas.width;
    
    xAxis = Math.floor(height/2);
    yAxis = Math.floor(width/4);
    
    context.save();
    draw();
}

/**
 * Draw animation function.
 * 
 * This function draws one frame of the animation, waits 20ms, and then calls
 * itself again.
 */
draw = function () {
    
    // Clear the canvas
    //context.clearRect(0, 0, width, height);
    context.fillStyle = "#fdf6e3";
    context.fillRect(0,0,500,300);


    // Draw the axes in their own path
    context.beginPath();
    
    context.stroke();
    
    // Set styles for animated graphics
    context.save();
    context.fillStyle = 'blue';
    context.fillText("Loading...",20,150);
    context.strokeStyle = '#00f';
    context.lineWidth = 2;
   
  
    // Update the time and draw again
    draw.seconds = draw.seconds - .007;
    draw.t = draw.seconds*Math.PI;
    drawSine(draw.t);
    drawinSine(draw.t);
    setTimeout(draw, 10);
	
	
};
draw.seconds = 0;
draw.t = 0;

 function drawSine(t) {
    context.beginPath();
    context.strokeStyle = 'orange';

    var x = t;
    var y = Math.sin(x);
  

    context.moveTo(yAxis, unit*y+xAxis);
    
    // Loop to draw segments
    for (i = yAxis; i <= width; i += 10) {
        x = t+(-yAxis+i)/unit;
        
    	y = Math.sin(x);
        context.lineTo(i, unit*y+xAxis);
		context.stroke();

	 }

 context.stroke();

} 

function drawinSine(t) {
    context.beginPath();
    context.strokeStyle = 'red';   
    var x = t;
    var y = -1 * Math.sin(x);
    context.moveTo(yAxis, unit*y+xAxis);
    
    for (i = yAxis; i <= width; i += 10) {
        x = t+(-yAxis+i)/unit;
        y = -1* Math.sin(x);
        context.lineTo(i, unit*y+xAxis);
	}
    context.stroke();
}





    Humble.Trig.init()
	    
})();

</script>

#Loading the animation into a gif

I had two approaches to converting the animation into a `.gif`. One approach
was run the conversion in javascript with a tool known as `jsgif`. The other approach was to use `byzanz`, a tool that
generate gifs from a screen capture.

#Using byzanz

I was able to find an extension of byzanz that actually lets you trace the
region you want to record <a href= "http://askubuntu.com/questions/107726/how-to-create-animated-gif-images-of-a-screencast"> here. </a>

{% highlight text linenos %}

byzanz 
FFcast2


{% endhighlight %}

Then copy this into a file called `byzanz-record-region.sh` and run it.

{% highlight bash linenos %}

#!/bin/bash

# Delay before starting
DELAY=10

# Sound notification to let one know when recording is about to start (and ends)
beep() {
    paplay /usr/share/sounds/KDE-Im-Irc-Event.ogg &
}

# Duration and output file
if [ $# -gt 0 ]; then
    D="--duration=$@"
else
    echo Default recording duration 10s to /tmp/recorded.gif
    D="--duration=10 /tmp/recorded.gif"
fi

# xrectsel from https://github.com/lolilolicon/FFcast2/blob/master/xrectsel.c
ARGUMENTS=$(xrectsel "--x=%x --y=%y --width=%w --height=%h") || exit -1

echo Delaying $DELAY seconds. After that, byzanz will start
for (( i=$DELAY; i>0; --i )) ; do
    echo $i
    sleep 1
done
beep
byzanz-record --verbose --delay=0 ${ARGUMENTS} $D
beep


{% endhighlight %}

The gif location would be `/tmp/recorded.gif`

<img src="/images/loading.gif" />

This was much more effective than the other approach. However, I would still
mention it as the quality may be improved in the future.

#Using jsgif

You can find the git repo <a href="https://github.com/antimatter15/jsgif">
here. </a>
First load the javascript files.

{% highlight html linenos %}
<script type="text/javascript" src="/assets/js/LZWEncoder.js"></script>
<script type="text/javascript" src="/assets/js/NeuQuant.js"></script>
<script type="text/javascript" src="/assets/js/GIFEncoder.js"></script>
<script type="text/javascript" src="/assets/js/b64.js"></script>

{% endhighlight %}

Then add the encoder to the javascript. I was a able to find a nice code segment that I was able to tweak
<a href="http://stackoverflow.com/questions/3346069/grabbing-each-frame-of-an-html5-canvas">
here. </a>

{% highlight javascript linenos %}

var w = setTimeout(function() { // give external JS 1 second of time to load

    console.log('Starting');

    var canvas = document.getElementById("sineCanvas");
    var context = canvas.getContext('2d');
    var shots  = [];
    var grabLimit = 60;  // Number of screenshots to take
    var grabRate  = 270; // Miliseconds. 500 = half a second
    var count     = 0;

    function showResults() {
        console.log('Finishing');
        encoder.finish();
        var binary_gif = encoder.stream().getData();
        var data_url = 'data:image/gif;base64,'+encode64(binary_gif);
        document.write('<img src="' +data_url + '"/>\n');
    }

    var encoder = new GIFEncoder();
    encoder.setRepeat(0);  //0  -> loop forever, 1+ -> loop n times then stop
    encoder.setDelay(0); //go to next frame every n milliseconds
    encoder.start();

    var grabber = setInterval(function(){
    console.log('Grabbing '+count);
    count++;

    if (count>grabLimit) {
        clearInterval(grabber);
        showResults();
      }

     encoder.addFrame(context);

    }, grabRate);

}, 1000);


{% endhighlight %}

<i class="fa fa-warning"></i>  **Notice:** Running the image capture on the
javascript itself actually interfered with the javascript code generating the
animation. It made the animation much slower. The only way to make the
animation faster is to reduce the rate of frame capture. This noticeably
reduces the quality of the animation. 
{: .notice}

<img src="/images/loadingalt.gif" />

