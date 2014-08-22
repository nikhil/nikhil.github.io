---
layout: post
title: Adding FontAwsome Icons to my resume using Latex 
code:
- type: packages
  count: 0

- type: file
  file: XeLaTeX
  count: 1

- type: file
  file: XeLaTeXe
  count: 2

- type: file
  file: XeLaTeXe
  count: 3

- type: bash
  count: 4

 
---
Yesterday I have decided to remodel my resume in latex. I decided that I would
like to use some of the icons from FontAwsome on my contact section to save
space. This was not as easy as expected, nevertheless I found a solution.

##Packages needed

{% highlight python linenos %}
fontspec
ttf-font-awesome
XeLaTeX or LuaTex

{% endhighlight %}

##Load the font

After installing the packages you can load the font by creating a file such as
`fontawesome.sty` that would be used as a LaTeX package.

{% highlight java linenos %}
\ProvidesPackage{fontawesome}
\usepackage{fontspec}
\newfontfamily{\FA}{FontAwesome}
{% endhighlight %}


After that, you need to map the symbols to its unicode. You can do that by
adding lines like this:


{% highlight java linenos %}
\def\faCamera{\symbol{"F030}}
\def\faFont{\symbol{"F031}}
\def\faBold{\symbol{"F032}}
...
{% endhighlight %}

Many of these maps cna be found online. One can be found <a
href="https://gist.github.com/sway/3101743">here.</a>


{% highlight java linenos %}
\documentclass{article}
\usepackage{fontawesome}
\begin{document}
\faCamera
\end{document}
{% endhighlight %}

<i class="fa fa-warning"></i> **Notice:** You will not be able to use
FontAwsome's fw option which help make lists with icons more aligned. A workaround would be to put icons and text in a tabular table with icons on one column and text in another.
{: .notice}

After finishing your resume you just need to run:

{% highlight TeX linenos %}
xelatex resume.tex
{% endhighlight %}


