---
layout: post
title: Designing dynamically generated html from Jekyll 
code:
- type: file
  file: Liquid
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


---
I have recently decided to change my code block to contain a header for the
programming language that I am using. However, I also use Jekyll to construct
my blog. `Jekyll` scans my `Markdown` files and dynamically generates code blocks
with highlighting from `Pygments`.
<br>
<br>
For example to display Ruby code. In my markdown I would write:

{% highlight javascript linenos %}

{% raw %}

{% highlight Ruby linenos %}


for current_iteration_number in 1..100 do
   puts "I have  #{current_iteration_number} problems-"
   puts "and Ruby is not one."
end


{% endhighlight %}
{% endraw %}

{% endhighlight %}

Will look like this:

{% highlight Ruby linenos %}


for current_iteration_number in 1..100 do
   puts "I have  #{current_iteration_number} problems-"
   puts "and Ruby is not one."
end


{% endhighlight %}


I wanted to create a title bar for my code block that would label the current
language it was showing.
However, since the code block is dynamically generated 
generated, it is hard to customize the block with just `css` alone. In addition,
I needed some way to inform the header of the language I am showing.
Jekyll with pygments generates the html for a code block. 
<br>
<br>
The ruby code will look like this:

{% highlight html linenos %}

<div class="highlight">
	<pre>
		<code class="language-ruby" data-lang="ruby">
		...
		</code>
	</pre>	
</div>	

{% endhighlight %}

I decided to create a container class so that I can position the title bar
using `relative/absolute positioning`.However. since Jekyll forms the html at compilation, I can't really modify the
code before its generated. This is where `Jquery` can be used.
<br>
<br>
For example, to create a Bash title bar, I would use:

{% highlight javascript linenos %}

$( document ).ready(function() {
     
       $("pre").addClass("terminal");
	   
		 $("<span>",{
	       rel:'  Bash'
		   }).appendTo("pre:eq(0)");

});	


{% endhighlight %}

The `pre:eq(0)` allows me to keep track of my code blocks where `0` is my
first and `1` is my second.
After the script the html should look like this:
{% highlight html linenos %}

<div class="highlight">
	<pre class="terminal">
		<code class="language-ruby" data-lang="ruby">
		...
		</code>
		<span rel="  Ruby"></span>	
	</pre>	
</div>	

{% endhighlight %}
<br>
Now I just need to edit the css of my added classes to form the title bar.
<br>
{% highlight css linenos %}



.terminal
{
position: relative;
}



span[rel]:before
{
content: attr(rel);
color: #343434;
font-family: Raleway, FontMfizz, FontAwesome;
font-size: 150%;
position: absolute;
top: 0;
left: 0;
width: 100%;
background: #CC7A00;
font-weight: normal;
font-style: normal;
padding: 5px 0;
text-indent: 15px;
border-radius: 4px 4px 0 0;

}

{% endhighlight %}

Now I have the title bar on my code blocks, however writing javascript for each
code blcok is messy and a pain. In my next post I will write about how I use
liquid to generate the javascript which generates the html selectors.



