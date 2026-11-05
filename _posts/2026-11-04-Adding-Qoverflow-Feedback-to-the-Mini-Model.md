# Adding $Q_{overflow}$ Feedback to the Mini Model

Last week, I did analytical work to get a sense of how my mini model will respond to changes in $Q_{in}$. I also want to understand how changes in $Q_{overflow}$ affect bed change. The big thing that is missing from this model is the actual feedback step: once we have a bed change, how is $Q_{overflow}$ modified?

## Simplifying dependency on $Q_{in}$

Last week, I solved an expression for $\frac{\Delta \eta}{\Delta t}$ as a function of $Q_{in}$. 

$$
\frac{\Delta \eta}{\Delta t}=C_5\left[(C_2Q_{in}^\frac{3}{5}-C_3)^\frac{3}{2}-(C_2(Q_{in}-Q_{over})^\frac{3}{5}-C_3)^\frac{3}{2}\right]
$$

Each constant in the equation represents properties of the channel or sediment that I am saying are fixed in the model. Specifically, $C_3$ is the nondimensional critical shear stress $\tau_c^*$, which equals 0.047. At high flows, which is what we are interested in, we would expect the bed shear stress from the $Q_{in}$ and $Q_{out}$ to dominate over $\tau_c^\ast$. We can identify a simplified equation where:

$$ 
\frac{\Delta \eta}{\Delta t}\sim Q_{in}^\frac{9}{10}-(Q_{in}-Q_{over})^\frac{9}{10}
$$

As $Q_{in}$ gets larger, $Q_{over}$ becomes insignificant and $\frac{\Delta \eta}{\Delta t}$ goes to zero. When holding $Q_{over}$ constant, we would expect the greatest rate of deposition to occur when $Q_{in}$ is closest in magnitude to $Q_{over}$, so that proportionally the difference between $Q_{in}$ and $Q_{out}$ is the greatest. Interestingly, the $q_s$ terms scale almost linearly with $Q$ because the exponent $\frac{9}{10}$ is close to 1. At first this result surprised me because my plotting of $\frac{\Delta \eta}{\Delta t}$ with respect to $Q_{in}$ seemed complicated. 

$$
q_{s,in} = C_1(C_2Q_{in}^\frac{3}{5}-C_3)^\frac{3}{2}
$$

Again, during a flood $\tau_{in} >> \tau_c^\ast$, where $\tau_c^\ast=C_3$ and $\tau_{in}=C_2Q_{in}^\frac{3}{5}$. Therefore we get the scaling relationship:

$$
q_{s,in}\sim Q_{in}^\frac{9}{10}
$$

## Understanding dependency on $Q_{out}$

If we hold $Q_{in}$ to be constant, then we can get at the relationship between $Q_{out}$ and $\frac{\Delta \eta}{\Delta t}$. We can start with the same equation for $\frac{\Delta \eta}{\Delta t}$:

$$ 
\frac{\Delta \eta}{\Delta t}\sim Q_{in}^\frac{9}{10}-(Q_{in}-Q_{over})^\frac{9}{10}
$$

which we can simplify by calling $Q_{in}=a$ because now we are considering it to be a constant and $Q_{over}=x$ because it is our dependent variable. 

$$ 
\frac{\Delta \eta}{\Delta t}\sim a^\frac{9}{10}-(a-x)^\frac{9}{10}
$$

In most cases that we are considering so far, $x << a$. In model runs so far I believe $Q_{over}$ is about 0.2 to 0.4 of $Q_{in}$, so this may only be a good assumption in some cases. If we can say that $x << a$, then we can see that the first and second terms in the equation are about the same because $x$ is small. This means that the two terms represent two points that are close together on the function $f(x) = x^\frac{9}{10}$. The derivative of $f(x)$ at $x=a$ can be approximated as:

$$ 
f\prime(x=a)\approx \frac{f(a)-f(a-\Delta x)}{\Delta x}
$$

Plugging in for $f()$:

$$ 
f\prime(x=a)\approx \frac{a^\frac{9}{10}-(a-\Delta x)^\frac{9}{10}}{\Delta x}
$$

Rearranging to get our equation for $\frac{\Delta \eta}{\Delta t}$:

$$
\frac{\Delta \eta}{\Delta t}\approx a^\frac{9}{10}-(a-\Delta x)^\frac{9}{10}\approx f\prime(x=a)\Delta x
$$

And substituting $f\prime(x=a)=\frac{9}{10}a^\frac{-1}{10}$

$$
\frac{\Delta \eta}{\Delta t}\approx a^\frac{9}{10}-(a-\Delta x)^\frac{9}{10}\approx \frac{9}{10}a^\frac{-1}{10}\Delta x
$$

This tells us that $\frac{\Delta \eta}{\Delta t}$ varies approximately linearly with $\Delta x$, which is the same as our $Q_{over}$ when $Q_{over}$ is small compared to $Q_{in}$. we can say that the rate of change of the bed elevation is linear with $Q_{over}$ when holding $Q_{in}$ constant and without making adjustments in time. 

## Adding in nonlinearity through the $Q_{over}$ feedback

Now that we understand the $\frac{\Delta \eta}{\Delta t}$ equation's dependence on $Q_{over}$ and $Q_{in}$ a little better, we can start adding in the feedback where $\frac{\Delta \eta}{\Delta t}$ modifies $Q_{over}$. Last week's discussion with Alex and Wuming led to me using the equation for weir flow to approximate flow from the channel onto the floodplain. This addition to the model adds a need for a timestep to solve for an absolute change in bed level during that timestep. 

The general equation for flow over a broad-crested weir is:

$$
Q_{over}=C_wLH^\frac{3}{2}
$$

where $H$ is the head of water above the weir crest, $L$ is the width of the crest, and $C_w$ is the weir coefficient. I think a broad-crested weir is the appropriate weir type in our case because topographic features act more as bumps with a significant depth to them. 

<img width="300" alt="image" src="https://github.com/user-attachments/assets/bd799dbc-3d24-465d-8ed8-806f59cbb348" />

Now we can focus on getting an absolute change in bed level. Conceptually, at $t_0$ we have an initial bed level and $Q_{over}$. All other parameters are constant in time. Then the following steps can take place.

1. Calculate the upstream flow depth using Mannings equation from $Q_{in}$ (this does not change in time, which is a major simplification in the model).
2. Calculate $Q_{over}$ at $t_0$ using the weir equation and calculating $H$ based on the flow depth from (1) and the elevation of the bed and the weir crest.
3. Calculate a $\Delta \eta$ using an ODE solver in MATLAB. Or, I can solve for $\Delta \eta$ by doing my own discretization. At each timestep, update the $Q_{over}$ before calculating the next bed change.

Next, I want to see if I can gain any insight into the nonlinearity of the feedback loop.

## A more analytical look at nonlinearity in the mini model

Let's express $Q_{over}$ in terms of $\eta$. If we substitute $H=\eta+y_{in}-h_{weir}$ into the weir equation, where $y_{in}$ is the flow depth from $Q_{in}$ we get:

$$
Q_{over}=C_wL(\eta+y_{in}-h_{weir})^\frac{3}{2}
$$

Now, let's substitute this equation in for $Q_{over}$ in our Exner equation, which is the first equation introduced in this blog. Let's say that $C_6=y_{in}-h_{weir}$. Then,

$$
\frac{\Delta \eta}{\Delta t}=C_5\left[(C_2Q_{in}^\frac{3}{5}-C_3)^\frac{3}{2}-(C_2(Q_{in}-Q_{over})^\frac{3}{5}-C_3)^\frac{3}{2}\right]
$$

becomes

$$
\frac{\Delta \eta}{\Delta t}=C_5\left[(C_2Q_{in}^\frac{3}{5}-C_3)^\frac{3}{2}-(C_2(Q_{in}-C_wL(C_6+\eta)^\frac{3}{2})^\frac{3}{5}-C_3)^\frac{3}{2}\right]
$$

From inspection of this equation, I do not see a way to get a simple power law that can describe the nonlinearity. However, we can use the conclusion of the argument that then $Q_{over}<<Q_{in}$, then $\frac{\Delta \eta}{\Delta t}$ is approximately linear with respect to $Q_{over}$. If we then substitute our weir equation in for $Q_{over}$, then we can get a simpler nonlinear relationship.

Recall from the section in this blog called "Understanding dependency on $Q_{out}$" we got the linear relationship below:

$$
\frac{\Delta \eta}{\Delta t}\approx a^\frac{9}{10}-(a-\Delta x)^\frac{9}{10}=\frac{9}{10}a^\frac{-1}{10}\Delta x
$$

where $\Delta x$ represents $Q_{over}$. If we sub in the weir equation for $\Delta x$

$$
\frac{\Delta \eta}{\Delta t}\approx \frac{9}{10}a^\frac{-1}{10}*C_wL(\eta+y_{in}-h_{weir})^\frac{3}{2}
$$

Ignoring all of the constants outside of the exponent:

$$
\frac{\Delta \eta}{\Delta t}\sim (\eta+y_{in}-h_{weir})^\frac{3}{2}
$$

So, my hypothesis is that the change in bed elevation should increase like the head over the weir crest to the power of $\frac{3}{2}$.

Next, I want to solve for $Q_out$ and see how it depends on $\eta$. 


