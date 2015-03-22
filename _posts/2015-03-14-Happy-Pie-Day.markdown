---
layout: post
comments: true
title: Happy Pie Day 
code:
- type: html
  count: 0
- type: file
  file: LaTex
  count: 1
- type: file
  file: LaTex
  count: 2
- type: file
  file: LaTex
  count: 3
- type: file
  file: LaTex
  count: 4
- type: file
  file: LaTex
  count: 5
- type: file
  file: LaTex
  count: 6
- type: file
  file: LaTex
  count: 7
- type: file
  file: LaTex
  count: 8
- type: file
  file: LaTex
  count: 9
 
---
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});
</script>
<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>


In honor of Pie day I decided to learn how to incorporate a math typeset onto
Html. The `mathjax` package makes this easy to do.

##Setting up the html


{% highlight html linenos %}

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});
</script>
<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

{% endhighlight %}

##Usage
You can now insert `LaTeX` equations in html by inserting them in the middle of two `$$` for an equation on a new line and two `$` for an equation on the same line.. 

<body>
<h2>Some cool $\pi$ stuff:</h2> 
<h4>Euler's identity:</h4>
$$e^{\pi i} = -1$$

{% highlight latex linenos %}

$$ e^{\pi i} = -1 $$


{% endhighlight %}


<h4>Heisenberg's uncertainty principle:</h4>
$$\Delta x \Delta p \geq \frac{h}{4\pi}$$

{% highlight latex linenos %}

$$\Delta x \Delta p \geq \frac{h}{4\pi}$$

{% endhighlight %}

<h4>Schrödinger Equation</h4>
$$\frac{ih}{2\pi} \frac{\partial}{\partial t} \Psi (x,t) = - \frac{h}{4\pi m}
\bigtriangledown^2 \Psi (x,t) + V(x) \Psi (x,t) $$ 

{% highlight latex linenos %}

$$\frac{ih}{2\pi} \frac{\partial}{\partial t} \Psi (x,t) = - \frac{h}{4\pi m}
\bigtriangledown^2 \Psi (x,t) + V(x) \Psi (x,t) $$ 


{% endhighlight %}

<h4>Integral approximation:</h4>
$$\int^\infty_{-\infty} \int^\infty_t  e^{-t^2 - \frac{1}{2}x^2 + xt} \,dx\,dt = \pi $$

{% highlight latex linenos %}

$$\int^\infty_{-\infty} \int^\infty_t  e^{-t^2 - \frac{1}{2}x^2 + xt} \,dx\,dt = \pi $$

{% endhighlight %}

<h4>Infinite Series approximation:</h4>
$$\sum_{n=1}^\infty \frac{3^n -1}{4^n} \zeta (n + 1) = \pi $$

{% highlight latex linenos %}

$$\sum_{n=1}^\infty \frac{3^n -1}{4^n} \zeta (n + 1) = \pi $$


{% endhighlight %}


<h2>Some cool engineering equations:</h2>

<h4>Normal Distribution:</h4>

$$\Phi (x) = \frac{1}{\sqrt{2\pi\sigma}} e^{\frac{(x-\mu)^2}{2 \sigma^2}}$$

{% highlight latex linenos %}


$$\Phi (x) = \frac{1}{\sqrt{2\pi\sigma}} e^{\frac{(x-\mu)^2}{2 \sigma^2}}$$


{% endhighlight %}

<h4>Fourier Transform</h4>

$$\int^\infty_{-\infty} f(x)e^{-2\pi i x \zeta} \,dx = f(\zeta) $$

{% highlight latex linenos %}

$$\int^\infty_{-\infty} f(x)e^{-2\pi i x \zeta} \,dx = f(\zeta) $$

{% endhighlight %}

<h4>Taylor Series</h4>

$$f(x) = f(a) + f^{\prime}(a)(x-a) + \frac{f^{\prime \prime}(a)}{2!} (x-a)^2...\frac{f^{n}(a)}{n!} (x-a)^n$$

{% highlight latex linenos %}

$$f(x) = f(a) + f^{\prime}(a)(x-a) + \frac{f^{\prime \prime}(a)}{2!} (x-a)^2...\frac{f^{n}(a)}{n!} (x-a)^n$$


{% endhighlight %}

</body>

 
