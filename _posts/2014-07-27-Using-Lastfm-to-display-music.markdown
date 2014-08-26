---
layout: post
comments: true
title: Using Lastfm to display music 
code:
- type: bash
  count: 0

- type: bash
  count: 1

- type: bash
  count: 2

---
After creating a music page, I have decided to take advantage of my knowledge of javascript to make the task of displaying my music selection easier. I wanted to create a script that would pull from a list of albums and display the album art, the year it was released, and the tags given to it from last.fm. You would need to first download the Lastfm package found <a href="https://github.com/fxb/javascript-last.fm-api
">here:</a>

Here is the basic overview of the api taken from the github page:
<blockquote>

The JavaScript last.fm API allows you to call last.fm API methods and get the corresponding JSON responses. Basically it just acts as a wrapper that gives easy access to API methods. Responses can be cached using the localStorage API.

</blockquote>

To use the api, you need to first register for the last.fm api keys.

{% highlight javascript linenos %}

/* Create a cache object */
var cache = new LastFMCache();
/* Create a LastFM object */
var lastfm = new LastFM({
  apiKey    : '',
  apiSecret : '',
  cache: cache

});

{% endhighlight %}

I then coupled javascript wiht yaml to retreieve my album info. I need to fix some dates that were incorrect or not given.

{% highlight javascript linenos %}

lastfm.album.getInfo({artist: '{{albumel.by}}',album: '{{albumel.name}}'}, {success: function(data){


var name = data.album.name;
var artist = data.album.artist;
var releasedate = data.album.releasedate;
{% if albumel.name == 'IV' %}
releasedate = '8 Nov 1971'
{% endif %}
 {% if albumel.name == 'Hot Space' %}
releasedate = '21 Nov 1981'
{% endif %}

var pieces = releasedate.split(",");
var date = pieces[0];
var tags = data.album.toptags.tag;
var tag;
var url = data.album.image[2]["#text"];
var albumimage = '<img src="'+url+'">';
var title = '<p>' + artist + ' - ' + name +'</p>';
var dateelem = '<p>' +'Released: '+ date + '</p>';

var html = title;
html += dateelem;
var codetag;
var tag;
var length = tags.length;
html += '<p>';
html += 'Tags: ';
for(var i=0; i<tags.length; i++)
{
   tag = tags[i];  
   codetag = '<code>'+ tag.name + '</code>';
   html += codetag;


}
html += '</p>';
html = '<div class="album"> <div class="albumimage">'+albumimage+'</div><div class="albumcontent">'+html+'</div></div>';
$('.albums').append(html);
}, error: function(code, message){
  
}});



{% endhighlight %}



