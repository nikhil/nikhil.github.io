---
layout: post
comments: true
title: Derivation of Flux from Concentration Profile 

---
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});
</script>
<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

<body>

After learning how to incorporate LaTeX into html. I decided to run through a
sample Biomedical equation derivation on the transport of a solute through time
`t` and space `z`.

<h3>What we know</h3>

Concentration Profile:
$$\bar{c}_{z,t} = \frac{c_{z,t} - c_L}{c_o - c_L} = 1- erf(\frac{z}{\sqrt{4Dt}})\tag{1}$$

Fick's first law:
$$ J = -D \frac{dc}{dz} \tag{2}$$

Error Function (erf):
$$ \frac{2}{\sqrt{\pi}} \int^n_0 e^{-n^2}  \tag{3}$$

<h3>Finding the Flux</h3>

<h4>We can start by simplifying the equation by setting:</h4>
$$ \frac{z}{\sqrt{4Dt}} = \eta \tag{4}$$

Then:
$$\frac{c_{z,t} - c_L}{c_o - c_L} = 1- erf(\frac{z}{\sqrt{4Dt}})$$
$$c_{z,t} - c_L = c_o - c_L -c_o erf(\eta) + c_L erf(\eta)$$
$$c_{z,t} = c_L + c_o - c_L -c_o erf(\eta) + c_L erf(\eta)$$
$$c_{z,t} = c_o + (-c_o +c_L) erf(\eta)\tag{5}$$

<h4>How to take the derivative of the erf function</h4>

$$\frac{d}{dn} \frac{2}{\sqrt{\pi}} \int^n_0 e^{-n^2}$$ 
$$  \frac{d}{dn}\frac{2}{\sqrt{\pi}}(erf(n) -erf(0)) $$
$$  \frac{d}{dn}\frac{2}{\sqrt{\pi}}(erf(n) -0) $$ 
$$  \frac{2e^{-n^2}}{\sqrt{\pi}}\tag{6}$$

<h4>Use equation (5) and (6) to take the derivative of the concentration
profile</h4>

$$\frac{d}{d{\eta}} = 0 + \frac{d}{d{\eta}}(-c_o +c_L) erf(\eta)$$
$$\frac{d}{d{\eta}} =  d{\eta}(-c_o +c_L) \frac{2e^{-\eta^2}}{\sqrt{\pi}} $$ 

<h4> Using equation (4) to substitute for $\eta$ </h4>

$$\frac{dc}{dz} =  (-c_o +c_L)\frac{1}{2\sqrt{Dt}} \frac{2e^{\frac{-z^2}{4Dt}}}{\sqrt{\pi}}$$
$$\frac{dc}{dz} =  (-c_o +c_L)\frac{e^{\frac{-z^2}{4Dt}}}{\sqrt{Dt\pi}}\tag{7} $$

<h4> Use equation (7) and (2) to find the flux </h4>

$$J =  -D(-c_o +c_L)\frac{e^{\frac{-z^2}{4Dt}}}{\sqrt{Dt\pi}} $$
$$J =  (c_o - c_L)\sqrt{\frac{D}{\pi t}}e^{\frac{-z^2}{4Dt}}\tag{8}$$














</body>

