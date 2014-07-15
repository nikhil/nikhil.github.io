---
layout: post
title: Monitoring the Hubble constant 
code:
- type: file
  file: Liquid
  count: 0

- type: file
  file: Liquid
  count: 1

---
Here is some work I did with the Hubble constant.
<div>
<button onclick="getData()" class="btn">Retrieve Hubble constant</button></p> 

</div>
<p class="constant">The Hubble constant:&nbsp; </p>
<div class="hubblechart">
<p class="head1"> Constellation</p> <p class ="head2"> Redshift (km/s)</p>
<p class="galaxy">
Virgo<br><br> Ursa Major <br><br> Corona Borealis <br><br> Boötes<br><br>Hydra

</p>
<p class="hubblediv"><span class="Hubble"></span></p>
</div>



<script type="text/javascript">

//$(document).ready(function(){
function getData() { 
    $.ajax({
        type: "GET",
        url: "http://en.wikipedia.org/w/api.php?action=parse&format=json&prop=text&page=Hubble's_law&callback=?",
        contentType: "application/json; charset=utf-8",
        async: false,
        dataType: "json",
        success: function (data, textStatus, jqXHR) {
            
			var markup = data.parse.text["*"];
            var blurb = $('<div></div>').html(markup);
            //$('article').html($(blurb).find('.wikitable td:eq(1)'));
			var str = $(blurb).find('.wikitable td:eq(1)');
			var elem = $(str).find('span:eq(1)');
			elem.remove();
			var bothnumstr =  $(str).find('span:eq(0)').html();
			var numstr = bothnumstr.substring(0,5);
			var num = parseFloat(numstr);
			$('.constant').append(num);
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
				
			//$('.Hubble').html(numstr);
        },
        error: function (errorMessage) {
        }
    });
}	
//});







</script>

