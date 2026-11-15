# Improving the Mini Model and Forming a Narrative for AGU

Last week, I generated initial results of my mini model using the default settings for the MATLAB `ode45` ODE solver. I want to play around with increasing the resolution in time to make the solutions smoother and see if I can decrease the error tolerance. 

## Improving Numerics of the ODE Solver

First, I investigated the default parameters of the MATLAB ODE solvers using the command `odeset` with no input parameters. The parameters that control the error are RelTol (relative tolerance), AbsTol (absolute tolerance), and NormControl (do/don't control the error relative to the norm of the solution rather than the absolute value).  In AMATH 581, we customized the absolute and relative tolerances rather than using the default values, which are listed below. 

```
>> odeset
          AbsTol: [ positive scalar or vector {1e-6} ]
          RelTol: [ positive scalar {1e-3} ]
     NormControl: [ on | {off} ]
     NonNegative: [ vector of integers ]
       OutputFcn: [ function_handle ]
       OutputSel: [ vector of integers ]
          Refine: [ positive integer ]
           Stats: [ on | {off} ]
     InitialStep: [ positive scalar ]
         MaxStep: [ positive scalar ]
         MinStep: [ positive scalar ]
             BDF: [ on | {off} ]
        MaxOrder: [ 1 | 2 | 3 | 4 | {5} ]
        Jacobian: [ matrix | function_handle ]
        JPattern: [ sparse matrix ]
      Vectorized: [ on | {off} ]
            Mass: [ matrix | function_handle ]
MStateDependence: [ none | {weak} | strong ]
       MvPattern: [ sparse matrix ]
    MassSingular: [ yes | no | {maybe} ]
    InitialSlope: [ vector ]
          Events: [ function_handle ]
```

The relative tolerance has a default value of $10^-3$ and the absolute tolerance has a default value of $10^-6$. These control the accuracy you want the solver to have at each timestep.

We also know that the Runge-Kutta 4,5 scheme has local truncation error of $\mathcal{O}(n) \Delta t^6)$ and global error (sum of the local truncation error) of $\mathcal{O}(n) \Delta t^5)$. This confused me at first because in my notes I have the `ode45` method written down as the 4,5 indicating a local error of $\mathcal{O}(n) \Delta t^4)$ and global error of $\mathcal{O}(n) \Delta t^5)$. However, it seems like the 4,5 actually refer to the two schemes that the method uses, and the final solution is based on the 5th order solution which has a local error of 6th order and global error of 5th order. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/996a0d24-a936-41bb-9d8b-d78f84ba470e" />





## Forming a narrative for AGU

