# Mini Conceptual Model of the Feedback Loop

In an effort to further understand the processes behind my result that larger floods do not cause a runaway feedback loop of overtopping, deposition in the main channel, loss of flow and sediment conveyance, and further overtopping, I started designing a mini model of the feedback loop. I am also hoping that this will be an interesting component of my AGU poster in early December. 

## Mini Model Setup - Conceptual

To construct my model, I started from the basics of bed change, which is the Exner equation. This is a simple mass balance that says that the mass of sediment moving into a control volume must equal the mass of sediment eroded (+ mass out) or deposited (- mass out) plus the mass that passes through.

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/d77a62fa-c94a-442d-959c-f76ca371c025" />

The key variable here is $\frac{\partial \eta}{\partial t}$, which I would use in my mini model to update the bed level in my control volume. The values I need to provide are $\lambda_p$, $\rho_p$, and $q_t$. $\lambda_p$ and $\rho_p$ are simple because I can just use the values for this that we use for the D50 in the model, which are wrapped up in the dry bed density and the specific gravity. The dry bed density is the density of a volume of a specific grain size when the pore space is entirely air (no water, hence "dry"). 

The harder part is of course obtaining the $q_t$, which is the driver of bed level change. See below for my conceptual drawing of the control volume. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/cf64a490-301b-4079-9fc2-0480c24f16b8" />

We think that in the Nooksack, changes in the inflow and outflow of sediment (sediment transport, deposition, erosion) have to do with the overflow dynamics. We also know that sediment transport is driven by shear stress on the bed, which is related to the flow in the channel. 

<img width="1503" alt="image" src="https://github.com/user-attachments/assets/b9f4382b-c85b-4be4-8225-cf1905396373" />

Q gives us a good starting point for the mini model. I could follow these steps:

1. Prescribe a $Q_{in}$, $Q_{out}$, and $Q_{overflow}$. These should be realistic to the model, and I can use discharge through my cross sections to get reasonable values. Hold $Q_{in} = Q_{out} + Q_{overflow}$ to be true. 
2. Using Manning's equation, convert my $Q_{in}$ and $Q_{out}$ to flow depths.
   $$Q=\frac{1}{n}R_H^{\frac{2}{3}}S_f^{\frac{1}{2}}$$

## Bringing back website blurb for review
