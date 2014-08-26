---
layout: post
comments: true
title: The Power of Liquid 
code:
- type: file
  file: Liquid
  count: 0

- type: file
  file: Liquid
  count: 1




---
After finding a way to edit my code blocks appearance. I ran into another
issue. Currently the only solution I have to fix the issue, is to run
javascript code on each of the code blocks. This is quite bothersome. However,
I found a way to neatly integrate the code block design in compilation by using
the language `liquid`.

### So what is liquid?
From their main page:
<blockquote>
Liquid is an open-source, Ruby-based template language created by Shopify. It is the backbone of Shopify themes and is used to load dynamic content on storefronts.
</blockquote>
Liquid allows me to make lists in my markdown files which I can then itterate
using a for each loop in liquid. For example, for my previous post my list was.

{% highlight yaml linenos %}

code:
- type: file
  file: Yaml
  count: 0

- type: ruby
  count: 1

- type: html
  count: 2 

- type: javascript
  count: 3

- type: html
  count: 4 

- type: css
  count: 5

{% endhighlight %}
Here there are 6 elements in `code` where each element begins with `-`.The
`type` tag specifies what language I am using. The `type file` is a dummy tag in which
the actual name is included in `file`. The `count` tag labels the code blocks
starting with `0` from the top.
<br>
<br>
With this list I can itterate through them using liquid.

{% highlight javascript linenos %}
{% raw %}
{% for codetag in page.code %}

{% if codetag.type == 'bash' %}

<script type="text/javascript">
$( document ).ready(function() {
     
       $("pre").addClass("terminal");
	   
		 $("<span>",{
	       rel:'  Bash'
		   }).appendTo("pre:eq({{codetag.count}})");

});	

</script>
{% endif %}

...

{% endfor %}

{% endraw %}

{% endhighlight %}

Where `codetag` loops through each item in `page.code`. Now, I only need to
mention the type on the top of my `markdown` file and liquid allows me to
generated the top bar of my code blocks with the proper label and icons.


