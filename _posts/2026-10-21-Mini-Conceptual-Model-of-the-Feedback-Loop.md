# Mini Conceptual Model of the Feedback Loop

In an effort to further understand the processes behind my result that larger floods do not cause a runaway feedback loop of overtopping, deposition in the main channel, loss of flow and sediment conveyance, and further overtopping, I started designing a mini model of the feedback loop. I am also hoping that this will be an interesting component of my AGU poster in early December. 

## Mini Model Setup - Conceptual

To construct my model, I started from the basics of bed change, which is the Exner equation. This is a simple mass balance that says that the mass of sediment moving into a control volume must equal the mass of sediment eroded (+ mass out) or deposited (- mass out) plus the mass that passes through.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/d77a62fa-c94a-442d-959c-f76ca371c025" />

The key variable here is $\frac{\partial \eta}{\partial t}$, which I would use in my mini model to update the bed level in my control volume. The values I need to provide are $\lambda_p$, $\rho_s$, and $q_t$. $\lambda_p$ and $\rho_s$ are simple because I can just use the values for this that we use for the D50 in the model, which are wrapped up in the dry bed density and the specific gravity. The dry bed density is the density of a volume of a specific grain size when the pore space is entirely air (no water, hence "dry"). 

The harder part is obtaining the $q_t$, which is the driver of bed level change. See below for my conceptual drawing of the control volume. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/cf64a490-301b-4079-9fc2-0480c24f16b8" />

We think that in the Nooksack, changes in the inflow and outflow of sediment (sediment transport, deposition, erosion) have to do with the overflow dynamics. We also know that sediment transport is driven by shear stress on the bed, which is related to the flow in the channel. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/b9f4382b-c85b-4be4-8225-cf1905396373" />

Q gives us a good starting point for the mini model. I could follow these steps:

1. Prescribe a $Q_{in}$, $Q_{out}$, and $Q_{overflow}$. These should be realistic to the model, and I can use discharge through my cross sections to get reasonable values. Hold $Q_{in} = Q_{out} + Q_{overflow}$ to be true. 
2. Using Manning's equation, convert my $Q_{in}$ and $Q_{out}$ to flow depths.

$$Q=\frac{1}{n}AR_H^{\frac{2}{3}}S_f^{\frac{1}{2}}$$

For normal flow, $S_f=S_0$, so I can use an estimate for the bed slope. I can also use a reasonable Mannings n value and approximate the hydraulic radius $R_H$ as the flow depth, which is only true for wide, shallow channels and might not be accurate for the Nooksack. Then:

$$h=(\frac{Qn}{bS_0^{\frac{1}{2}}})^{\frac{3}{5}}$$

3. With $h$ at the inlet and outlet of the control volume, I can derive a bed shear stress using the following:

$$\tau=\gamma hS_0$$

where $h$ is approximating $R_H$. 

4. Now that we have the bed shear stress, we can use one of the sediment transport formulas. I chose Meyer-Peter-Muller because it is simple and still widely used. (And I have notes on it from my master's!)

$$
q_s^* = 8(\tau^* - \tau_c^*)^{3/2}
$$

Here, the critical shear $\tau_c^\ast=0.047$ and $\tau^*$ is the nondimensional bed shear stress:

$$
\tau^* = \frac{\tau_b}{(\rho_s - \rho) g d_s}
$$

I will set $d_s$ to the $D_{50}$ from this figure:

<img width="700" alt="image" src="https://github.com/user-attachments/assets/75cbb670-02bc-4414-b62e-11abadcff9c2" />

At the Everson gage, the $D_{50}$ is about 40-50 mm. 

5. We now have everything we need in order to use the Exner equation.

$$
(1-\lambda_p)\frac{\partial \eta}{\partial t}=-(\frac{q_{out}-q_{in}}{\delta x})  
$$

{:start="6"}
6. I can then update the bed level in my box and restart from step 1 with a higher $Q_{overflow}$.

Once I start coding this process up, I am sure that I will come across road bumps and new ideas and have to adjust the method. I want to slowly make the mini model more complex and sophisticated while still staying very conceptual and simplistic. This is just the bare bones of it. 

## Bringing back website blurbs for review

River Research Blurb

> We are particularly interested in the dynamics of the rivers from their interface with the ocean to the mountain front. Our areas of study span topics like transport dynamics in river plumes, compound flooding, sediment transport and channel change, hydro-morphodynamic feedback during flooding, and river avulsion.

Nooksack Research Blurb

> The Nooksack River in northwestern Washington is a dynamic river system defined by a post-glacial landscape, sediment loads from the volcano Mount Baker, intensifying atmospheric rivers, and human engineering for flood control. To add to the Nooksack's complexity, in the late Holocene, the river occupied a different flow path than it does currently. The process that allowed the river to switch to its present-day course is called avulsion, which occurs when a river rapidly changes course. In the present-day lower Nooksack, extreme flooding forces water out of the main channel and into surrounding floodplains, and some of this floodwater is routed through the old Nooksack valley, raising questions about the potential for another river avulsion. We are using two-dimensional hydromorphodynamic modeling with Delft3D FM to address two goals: 1) we want to accurately model this complex region and use our model to support engineering solutions for the massive flooding, and 2) we want to understand if an avulsion is possible on the decadal time scale and understand the processes that govern avulsion in the Nooksack Valley. This work aims to improve our understanding of river response to extreme flooding and support the local communities in the Nooksack River floodplain.

Revised Nooksack Research Blurb
>The Nooksack River in northwestern Washington is a dynamic river that is prone to flooding, which reshapes the riverbed and sometimes causes substantial damage to surrounding communities. Its behavior is shaped by a landscape formed after the last ice age, sediment from Mount Baker, atmospheric rivers, and decades of human efforts to control flooding. To add to the Nooksack’s complexity, in the late Holocene, the river rapidly changed course in a process known as *avulsion*. In the present-day lower Nooksack, extreme flooding forces water out of the main channel and into surrounding floodplains, and some of this floodwater is routed through the river's historical path, raising questions about the potential for another river avulsion. We are using two-dimensional hydromorphodynamic modeling with Delft3D FM to address two goals: 1) accurately model this complex region and use our model to support engineering solutions for the flooding, and 2) understand if a future avulsion is possible on the decadal time scale and understand the processes that govern avulsion in the Nooksack. Ultimately, this work aims to improve our understanding of river response to extreme flooding and support the local communities in the Nooksack River floodplain.



