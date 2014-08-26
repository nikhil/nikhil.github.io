---
layout: post
comments: true
title: Monitoring the Hubble constant with Ajax 
code:
- type: file
  file: javascript
  count: 0

- type: file
  file: html
  count: 1

---
So lately I was contemplating on creating a project using Ajax. I decided that
I wanted to do something really neat with data that is constantly changing. The first thing that came to mind was the `Hubble constant`. The Hubble constant is used to calculate the recessional velocity or the redshift of many different Nebulas. The Hubble constant is used in the Hubble law in the relation:
<blockquote>
v = HR
</blockquote>
V is the velocity and R is the current distance from Earth. The Hubble constant is not actually constant as the universe continues to
expand. Here is some work I did with the Hubble constant using Ajax.

<p><button onclick="getData()" class="btn">
Retrieve Hubble constant</button></p> 



<p>The Hubble constant:&nbsp;<span id="hubble"></span></p>
<div class="hubblechart">
<p class="head1"> Constellation <hr></p> <p class ="head2"> Redshift (km/s)</p>
<p class="galaxy">
Virgo<br><br> Ursa Major <br><br> Corona Borealis <br><br> Boötes<br><br>Hydra

</p>
<p class="hubblediv"><span class="Hubble"></span></p>

</div>


<script type="text/javascript">

//$(document).ready(function(){
var clicked = false;
function getData() {
if(!clicked)
{  clicked = true;
    $.ajax({
        type: "GET",
        url: "http://en.wikipedia.org/w/api.php?action=parse&format=json&prop=text&page=Hubble's_law&callback=?",
        //contentType: "application/json; charset=utf-8",
		dataType: "json",
        success: function (data) {
            
			var parsed = data.parse.text["*"];
            var page = $('<div></div>').html(parsed);
        	var str = $(page).find('.wikitable td:eq(1)');
			var elem = $(str).find('span:eq(1)');
			elem.remove();
			var bothnumstr =  $(str).find('span:eq(0)').html();
			var numstr = bothnumstr.substring(0,5);
			var num = parseFloat(numstr);
			$('#hubble').append(num);
			$('#hubble').addClass("constant")
            var distance = ["23.92638037","306.7484663","429.4478528","766.8711656","1226.993865"];

			for(i=0; i< distance.length; i++)
			{

				var dist = parseFloat(distance[i])*num;
				var pelem = document.createElement("p");
				var txtelem = document.createTextNode(dist);
				console.log(txtelem);
				var insert = $('<p class = "result"></p>').append(txtelem);
				console.log(insert);
				$('.Hubble').append(txtelem);
	      		$('.Hubble').append('<br>');
        		$('.Hubble').append('<br>');





			}
				
	   }
        
    });

}	
}	








</script>

Getting Ajax to work on wikipedia was actually quite tricky. There were
numerous potential solutions to extract a page from wikipeida. Eventually
extensive trial and error showed me a correct answer. This type of reference to
a page in wikipedia is known as a `Cross-domain` reference. This conflicts with
the `Same-origin polic` for javascript which states that the resource domain
must be the same as the target domain. This is where `JSON-P`
(JSON-with-padding) comes in allowing us to receive and process data
cross-domain. 
<br>
<br>
Here is how my Ajax request looked:

{% highlight java linenos %}
function getData() {
  if(!clicked)
  {clicked = true;
  $.ajax({
        type: "GET",
        url: "http://en.wikipedia.org/w/api.php?action=parse&format=json&prop=text&page=Hubble's_law&callback=?",
        contentType: "application/json; charset=utf-8",
        dataType: "json",
        success: function (data) 
		{

		var parsed = data.parse.text["*"];
		var page = $('<div></div>').html(parsed);
		var str = $(page).find('.wikitable td:eq(1)');
		var elem = $(str).find('span:eq(1)');
		elem.remove();
		var bothnumstr =  $(str).find('span:eq(0)').html();
		var numstr = bothnumstr.substring(0,5);
		var num = parseFloat(numstr);
		$('#hubble').append(num);
		$('#hubble').addClass("constant");

		...

		},
		error: function (errorMessage) 
		{
        
		...

		}

  });
  }
} 

{% endhighlight %}

This combs through the Wikipedia page given by the Ajax command and finds the
Hubble constant form the research paper logs. It then converts it into a
number, displays by identifying the id  Hubble in the `DOM`. It then adds the
element with a new class to add some styling to the number.

{% highlight html linenos %}
<p><button onclick="getData()" class="btn">
Retrieve Hubble constant</button></p> 

<p>The Hubble constant:&nbsp;<span id="hubble"></span></p>

{% endhighlight %}



