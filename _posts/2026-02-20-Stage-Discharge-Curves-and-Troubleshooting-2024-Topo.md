## Stage-Discharge Curves and Troubleshooting 2024 Topo

This week, I wrote code to plot the USGS rating curves alongside the water surface elevation and discharge data from the model to see how well they compare. I also started thinking about the next steps with roughness, which include processing the 2024 NLCD (land cover data) and iteratively running the model to get a converging $\tau$, $D_{50}$, and Mannings $n$. 

### Stage-Discharge Curves

I wrote a new function to scrape the USGS rating curve data and plot it alongside stage and discharge from the model. 

```matlab
