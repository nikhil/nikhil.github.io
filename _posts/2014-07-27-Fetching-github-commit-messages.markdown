---
layout: post
comments: true
title: Fetching Github Commit Messages 
code:
- type: javascript 
  count: 0
  
---

I thought of an idea that will be pretty unique for my website. Since my
website is hosted on `github`, I would like to access the commit messages to my
git repository and display them to show the changes that are being made and
also to make the point that this website is constantly being developed and
tweaked by me.

Initially I thought it would be a good idea to use the javascript wrapper to
access the github api. However, that made things more confusing. So I was able
to retrieve the messages with just a simple get request.


{% highlight bash linenos %}
$.get(
"https://api.github.com/repos/nikhil/nikhil.github.io/commits",
function(data){
   	var date;
	var datepart;
	var messagestr;
	var html = '<br><span class = "changeitems">';
	
	for(i=0;i < 8; i++)
	{	
		
	messagestr = data[i].commit.message;
	date = data[i].commit.committer.date;
	datepart = date.split("T")[0];
	html += '<font color = "#586e75">'+ datepart.substring(5)
	+'</font>'+'&nbsp;&nbsp;'+messagestr+ '<br>';

				        
			
			
	} 

		html += '</span>';
		$('.changelog').append(html);
	}




)

{% endhighlight %}

Notice the format of the url. It goes 


https://api.github.com/repos/`user`/`repo`/commits.


