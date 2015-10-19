---
layout: post
comments: true
title: Hermite and Catmull Rom Splines
code:
- type: file
  file: C3d File
  count: 0

- type: packages
  count: 1

- type: bash
  count: 2

- type: bash
  count: 3

- type: python
  count: 4

- type: python
  count: 5

- type: python
  count: 6

- type: python
  count: 7

- type: python
  count: 8
 
---
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});
</script>
<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

In this post we will discuss the implementation of Hermite and Catmull Splines in <a href="https://github.com/SteerSuite/SteerLite">SteerLite.</a> The projects were created by a team of 3 as part of a class project for Introduction to Computer Graphics at Rutgers University.

## Hermite Videos

Curve 1 

<iframe width="480" height="270" src="https://www.youtube.com/embed/p2cBmDHkX8M" frameborder="0" allowfullscreen></iframe>

Curve 2

<iframe width="480" height="270" src="https://www.youtube.com/embed/EQaCPhRQ5Kw" frameborder="0" allowfullscreen></iframe>

Curve 3

<iframe width="480" height="270" src="https://www.youtube.com/embed/3nyAePrheYk" frameborder="0" allowfullscreen></iframe>

Curve 4

<iframe width="480" height="270" src="https://www.youtube.com/embed/1CMfTxH-soI" frameborder="0" allowfullscreen></iframe>

Original Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/ayQpalBsPVY" frameborder="0" allowfullscreen></iframe>

###Equation

$$ p(x) = h_{00}(t)p_k + h_{10}(t)(x_{k+1} - x_{k})m_k + h_{01}(t)p_{k+1} + $$
$$ h_{11}(t)(x_{k+1}-x_{k})m_{k+1} $$ 

$$ t = \frac{(x-x_k)}{x_{k+1}-x_k} $$ 

$$h_{00}(t) = 2t^3 -3t^2 +1$$

$$h_{10}(t) = t^3 -2t^2 +t $$

$$h_{01}(t) = -2t^3 +3t^2 $$

$$h_{11}(t) = t^3 - t^2 $$

##Catmull Rom Videos

Curve 1

<iframe width="480" height="270" src="https://www.youtube.com/embed/NuIq5d_gjwI" frameborder="0" allowfullscreen></iframe>

Curve 2

<iframe width="480" height="270" src="https://www.youtube.com/embed/FK6c0aGL2kA" frameborder="0" allowfullscreen></iframe>

Curve 3

<iframe width="480" height="270" src="https://www.youtube.com/embed/0d7vB21awEs" frameborder="0" allowfullscreen></iframe>

Curve 4

<iframe width="480" height="270" src="https://www.youtube.com/embed/-gzJq5ZUeTw" frameborder="0" allowfullscreen></iframe>

Original Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/sWplE-kpviM" frameborder="0" allowfullscreen></iframe>

###Equation

$$ p(x) = h_{00}(t)p_k + h_{10}(t)(tangent_{0})m_k + h_{01}(t)p_{k+1} + $$
$$ h_{11}(t)(tangent_{1})m_{k+1} $$ 

$$ t = \frac{(x-x_k)}{x_{k+1}-x_k} $$

$$h_{00}(t) = 2t^3 -3t^2 +1$$

$$h_{10}(t) = t^3 -2t^2 +t $$

$$h_{01}(t) = -2t^3 +3t^2 $$

$$h_{11}(t) = t^3 - t^2 $$

Current point is the next point you will reach

$$ Tangent_0 $$ has i = currentPoint -1

$$ Tagent_1 $$ has  i = currentPoint

If First Segment of Curve:

$$ FirstCoef = \frac{(t_2 - t_{0})}{(t_{2} - t_{1})}$$

$$ FirsTerm = \frac{(p_{1} - p_0)}{ (t_{1} - t_0)} $$

$$ SecondCoef = -1 * \frac{(t_{1} - t_0) }{(t_{2} - t_{1})}$$

$$ SecondTerm = \frac{(p_2 - p_{0})}{(t_2 - t_{0})} $$

$$ Tangent_0 = firstCoeff * firstTerm + $$ 

$$secondCoeff * secondTerm $$

If Last Segment of Curve:

$$ FirstCoef = \frac{(t_i - t_{i-})}{(t_{i-1} - t_{i-2})}$$

$$ FirsTerm = \frac{(p_{i} - p_{i-1})}{ (t_{i} - t_{i-1})} $$

$$ SecondCoef = -1 * \frac{(t_{i} - t_{i-1}) }{(t_{i-1} - t_{i-2})}$$

$$ SecondTerm = \frac{(p_i - p_{i-2})}{(t_i - t_{i-2})} $$

$$ Tangent_0 = firstCoeff * firstTerm + $$ 

$$secondCoeff * secondTerm $$

Any Segment between First and Last:

$$ FirstCoef = \frac{(t_i - t_{i-1})}{(t_{i+1} - t_{i-1})}$$

$$ FirsTerm = \frac{(p_{i+1} - p_i)}{ (t_{i+1} - t_i)} $$

$$ SecondCoef = \frac{(t_{i+1} - t_i) }{(t_{i+1} - t_{i-1})}$$

$$ SecondTerm = \frac{(p_i - p_{i-1})}{(t_i - t_{i-1})} $$

$$ Tangent_0 = firstCoeff * firstTerm + $$ 

$$secondCoeff * secondTerm $$

Github link: <a
href="https://github.com/CG-F15-9-Rutgers/SteerLite/blob/master/steerlib/src/Curve.cpp">
Here </a>


 
