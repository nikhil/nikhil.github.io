---
layout: post
comments: true
title: How to make your own blog.
code:
- type: bash
  count: 0

- type: bash
  count: 1

- type: bash
  count: 2

- type: bash
  count: 3

- type: bash
  count: 4

- type: bash
  count: 5

- type: bash
  count: 6

- type: bash
  count: 7

- type: bash
  count: 8
 
---
Creating a blog is not that hard. I would say, the hardest part would be
getting started. In addition, if you do not want a `custom domain` (more of
that in a bit) then you can get your blog up and running for free.

##Services that we would use

We will be using two main services to create and host our blog.I will discuss
how to use these services in my post.
Our blog will be created with `Jekyll` and hosted by `Github Pages`.
<blockquote>
<a href="http://jekyllrb.com">Jekyll</a><br>
<a href="https://pages.github.com">Github Pages</a>
</blockquote>

##Jekyll

<img src="../images/jekyll.png" alt="Jekyll">

Jekyll converts plain text and `markdown` files to create the Html for you. In
addition. it uses `Liquid` for creating cool templates. Check out my post on
how I used liquid and javascript to create my code blocks <a
href="http://kumarcode.com/The-Power-of-Liquid/">here. </a>

##Installing Jekyll

You would need `Ruby` to run jekyll.
{% highlight java linenos%}

sudo pacman -S ruby

{% endhighlight %}

You will also need to make sure `python2` is installed

{% highlight java linenos%}

sudo pacman -S python2

{% endhighlight %}

Now download Jekyll from the `AUR`.

{% highlight java linenos%}

pacaur -S ruby-jekyll

{% endhighlight %}

As of lately, it seems the AUR  Jekyll package cannot see the dependencies
If this happens to you, you can directly install the dependencies using `gem`

{% highlight java linenos%}

gem install jekyll

{% endhighlight %}

Cool now you have jekyll installed.

##Getting a template

There are a lot of awesome Jekyll templates that you could use. 
Check out the gallery <a href="http://jekyllthemes.org/"> here. </a>

###Using my website layout

If you are interested in using my theme you can `fork` it off my github repo.

1. Login or create an account on <a href="https://github.com/">Github.</a>
2. Visit my code <a href="https://github.com/nikhil/nikhil.github.io">here</a>
3. Click the `Fork` button on the upper right.

Now you have a working (hopefully) jekyll repo. Cool!

###Starting from scratch

If you decided that you want to just start from scratch you can init a new
jekyll blog.

{% highlight bash linenos%}
jekyll new MyWebsite

{% endhighlight %}

Now you can add `MyWebsite` to your github repo.

##Using Github Pages to host your repo

<img src="../images/github.png" alt="Github Pages">

Yup, Github can host this repo for you for free.

You will need to rename this repo. To let github know that you want to host
this repo it should be named `[username].github.io`. Just for an example, my username is `nikhil` so
my repo name is `nikhil.github.io`.

###Get a copy of your repo

Your website would be hosted on the url
`[username].github.io`, but first you need to modify/remove the `CNAME` file. This file is used for custom domain which we will cover in a bit.

Replace `[username]` with your username.

{% highlight bash linenos%}
mkdir MyWebsite
cd MyWebsite
git init
git clone https://github.com/[username]/[username].github.io
{% endhighlight %}

###Let Github know if you have a custom domain

If you have a custom domain you should remove my domain from `CNAME` and add in
your one. If you do not have a custom domain, you should just remove the file,
as shown below.

{% highlight bash linenos%}
rm CNAME
git add .
git commit -m "Removed CNAME"
git push origin master
{% endhighlight %}

##Custom domains

The url for your website is currently `[username].github.io`. If you want to
have a custom domain you will need to purchase a domain from a `domain
registrar` such as Namecheap.

###Enter your modify domain page

For Namecheap, you can do this by going to your domain page and click on `All
Host Records`. Remember to replace `[username]` with your Github username.

<table>
<tbody>
<tr>
<td>
Host Name
</td>
<td>
IP ADDRESS/URL
</td>
<td>
RECORD TYPE
</td>
<td>
TTL
</td>
</tr>
<tr>
<td>
@
</td>
<td>
192.30.252.154
</td>
<td>
A(Address)
</td>
<td>
1800
</td>
</tr>
<tr>
<td>
www
</td>
<td>
[username].github.io.
</td>
<td>
CNAME (Alias)
</td>
<td>
1800
</td>
</tr>
</tbody>
</table>

##Testing

Any changes that you push to github will update your current website. 
You can test your website at anytime before pushing. 

{% highlight bash linenos%}
cd MyWebsite
jekyll serve
{% endhighlight %}

You can now see your website by going to `http://localhost:4000/` on your
browser. 











