# Adding $Q_{overflow}$ Feedback to the Mini Model

Last week, I did analytical work to get a sense of how my mini model will respond to changes in $Q_{in}$. I also want to understand how changes in $Q_{overflow}$ affect bed change. The big thing that is missing from this model is the actual feedback step: once we have a bed change, how is $Q_{overflow}$ modified?

## Simplifying dependency on $Q_{in}$

Last week, I solved an expression for $\frac{\Delta \eta}{\Delta t}$ as a function of $Q_{in}$. 

$$
\frac{\Delta \eta}{\Delta t}=C_5\left[(C_2Q_{in}^\frac{3}{5}-C_3)^\frac{3}{2}+(C_2(Q_{in}-Q_{over})^\frac{3}{5}-C_3)^\frac{3}{2}\right]
$$

Each constant in the equation represents properties of the channel or sediment that I am saying are fixed in the model. Specifically, $C_3$ is the nondimensional critical shear stress $\tau_c^*$, which equals 0.047. At high flows, which is what we are interested in, we would expect the bed shear stress from the $Q_{in}$ and $Q_{out}$ to dominate over $\tau_c^\ast$. We can identify a simplified equation where:

$$ 
\frac{\Delta \eta}{\Delta t}\sim Q_{in}^\frac{9}{10}-(Q_{in}-Q_{over})^\frac{9}{10}
$$

As $Q_{in}$ gets larger, $Q_{over}$ becomes insignificant and $\frac{\Delta \eta}{\Delta t}$ goes to zero. When holding $Q_{out}$ constant, we would expect the greatest rate of deposition to occur when $Q_{in}$ is closest in magnitude to $Q_{over}$, so that proportionally the difference between $Q_{in}$ and $Q_{out}$ is the greatest. Interestingly, the $q_s$ terms scale almost linearly with $Q$ because the exponent $\frac{9}{10}$ is close to 1. At first this result surprised me because my plotting of $\frac{\Delta \eta}{\Delta t}$ with respect to $Q_{in}$ seemed complicated. 

$$
q_{s,in} = C_1(C_2Q_{in}^\frac{3}{5}-C_3)^\frac{3}{2}
$$

Again, during a flood $Q_{in}^\frac{3}{5} >> \tau_c^\ast$, where $\tau_c^\ast=C_3$. Therefore we get the scaling relationship:

$$
q_{s,in}\sim Q_{in}^\frac{9}{10}
$$

If we hold $Q_{out}$ to be constant, then we get the result:

$$ 
\frac{\Delta \eta}{\Delta t}\sim Q_{in}^\frac{9}{10}-(Q_{in}-Q_{over})^\frac{9}{10}
$$

which we can simplify by calling $Q_{in}=a$ because now we are considering it to be a constant and $Q_{over}=x$ because it is out dependent variable. 

$$ 
\frac{\Delta \eta}{\Delta t}\sim a^\frac{9}{10}-(a-x)^\frac{9}{10}
$$

In most cases that we are considering so far, $x << a$. In model runs so far I believe $Q_{over}$ is about 0.2 to 0.4 of $Q_{in}$, so this may only be a good assumption in some cases. If we can say that $x << a$, then we can see that the first and second terms in the equation are about the same because $x$ is small. This means that the two terms represent two points that are close together on the function $f(x) = x^\frac{9}{10}$. The derivative of $f(x)$ at $x=a$ can be approximated as:

$$ 
f\prime(x=a)=\frac{f(a)-f(a-\Delta x)}{\Delta x}
$$

Plugging in for $f()$:

$$ 
f\prime(x=a)=\frac{a^\frac{9}{10}-(a-\Delta x)^\frac{9}{10}}{\Delta x}
$$

Rearranging to get our equation for $\frac{\Delta \eta}{\Delta t}$:

$$
\frac{\Delta \eta}{\Delta t}=a^\frac{9}{10}-(a-\Delta x)^\frac{9}{10}=f\prime(x=a)\Delta x
$$

And substituting $f\prime(x=a)=\frac{9}{10}a^\frac{-1}{10}$

$$
\frac{\Delta \eta}{\Delta t}=a^\frac{9}{10}-(a-\Delta x)^\frac{9}{10}=\frac{9}{10}a^\frac{-1}{10}\Delta x
$$

This tells us that $\frac{\Delta \eta}{\Delta t}$ varies approximately linearly with $\Delta x$, which is the same as our $Q_{over}$ when $Q_{over}$ is small compared to $Q_{in}$. 
