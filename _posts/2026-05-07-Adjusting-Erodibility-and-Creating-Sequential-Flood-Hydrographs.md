# Adjusting Erodibility and Creating Sequential Flood Hydrographs

This week I have the following items on my list:

1. Remove the bridges from the non-erodible areas and rerun the "nonerodible" models with the updated sediment thickness files. 
2. Run morpho off models for all durations.
3. Start thinking about a more realistic way to adjust floodplain erodibility.
4. Work on a more comprehensive plot of bed change vs. river station.
5. Look into changing the shape of the 100-year storm based on the Deming 100-year hydrograph from Whatcom County.
6. Start EFM presentation.
7. Start application for NextProf Nexus. 

# Removing the Bridges

With some ArcGIS Pro work, I was able to edit the pixels in the original NLCD raster to reclassify the bridges (Developed land use type) to the Open Water designation. Then I reran my script that adds the nonerodible areas to the sediment fractions, essentially making those areas have 0 meters of erodible material (sediment thickness). While I was moving through the NLCD raster in ArcGIS Pro, I noticed that a future modification would be to have the Developed, Open Space category to erodible. It seems like a lot of this land is just open fields that are nearby development. Since they do not have pavement or hard built structures, this should probably be nonerodible. The first two figures in this blog show the before and after of the bridge removal. Notice that for the Everson corridor, Everson Bridge used to be represented as non-erodible. 

<img width="1396" alt="image" src="https://github.com/user-attachments/assets/a4bdb46b-e375-40f8-be75-82d33be7449c" />

*Figure 1. Sediment Fractions at the Everson Corridor with Non-Erodible Areas at the Bridges*

<img width="1392" alt="image" src="https://github.com/user-attachments/assets/6cc094df-b9f0-4b66-822f-76e7e5dec527" />

*Figure 2. Sediment Fractions at the Everson Corridor with Non-Erodible Areas Removed at the Bridges*

# Additional Model Runs

Addition model runs for morpho off and no bridges have been run!

# Adjusting Erodibility

### Thoughts on Developed, Open Space

I started this task by reviewing the [NLCD land cover class descriptions](https://www.mrlc.gov/data/legends/national-land-cover-database-class-legend-and-description#:~:text=Developed%2C%20Open%20Space%2D%20areas%20with,erosion%20control%2C%20or%20aesthetic%20purposes.) especially for the Developed, Open Space category. This is a confusing category because it seems to cover the smaller, rural roads (still paved, not dirt or gravel) and vegetated areas like golf courses, backyards, fields next to developement, and creek corridors through small towns. If we set this land type to nonerodible, which is what we have been doing, we are overestimating the amount of nonerodible area. 

<img width="619" alt="image" src="https://github.com/user-attachments/assets/12ff6444-b318-4317-9f74-1d69b8c190da" />

*Figure 3. Land Cover Classification Descriptions for Developed Types*

This description reflects what I opserved of the open space type in the raster. See Figures 4 and 5 for a look at the raster in the overflow path. Because the pixels are 30x30m, the roads take up less than 20% of a pixel. We could set this area to be erodible, but then some sections of paved road would be lost, and we think that those might be important to represent in the model as they should act as high points in the model that prevent erosion in the overflow path. We could try and find a way to designate them based on a different, more accurate raster of roads in Whatcom County. Maybe this is something that Wuming could try changing in his model to see if it makes a difference on the calibration metrics. For now I will leave it as nonerodible. 

<img width="1383" alt="image" src="https://github.com/user-attachments/assets/62284a30-53e5-42c4-b102-4561ebbf07cc" />

*Figure 4. Land Cover Raster North of Everson Corrider.*

<img width="1385" alt="image" src="https://github.com/user-attachments/assets/168e8e1b-acfc-486a-9ff5-ba0a2c2251aa" />

*Figure 5. Aerial Imagery of the Same Area Shown in Figure 4.*

### Floodplain Sediment Fractions

The next metric I wanted to look at is the floodplain sediment fractions for the areas left as erodible. In my research plan, I envisioned globally increasing or decreasing the sediment sizes on the floodplain, which would increase or decrease the critical shear stress required to move the sediment. This idea was somewhat based on a paper I had read by [Hajek and Edmonds, 2014](https://watermark02.silverchair.com/199.pdf?token=AQECAHi208BE49Ooan9kkhW_Ercy7Dm3ZL_9Cf3qfKAc485ysgAAAzYwggMyBgkqhkiG9w0BBwagggMjMIIDHwIBADCCAxgGCSqGSIb3DQEHATAeBglghkgBZQMEAS4wEQQMiGcdT6IGaf4xbtLjAgEQgIIC6SGfSweq3GxirQ-u_5VwQbCo4HvbEcDcTFdtLWmrQrePjdmAGHjuhYtbCC49vPLRtFnUmhaXXzK1Q1mVKH6HaQgA5UvepeyCiEQiwTYvSaSCPHUqM-dS_DxNEJZk4BcmASzBkhbVRg45jtYjw356Zzd9NpY5WqavclXVVJQm0xTQkiJVKkqoKV4__FjcelVDspRy88Fu67pjXxGCXvDFp6HPfD7H7mOyhvQnj_hy5Kpd5Pjqg_6kSelwO4IPQDAsM6vhobtFuEx79_mI1KwGs-KsC59YV-IjlMGETvXBxlRz0nyKxAt5sROnv7AIJTO3ZdbDCI8SBujnJaIKDVoNNIc-0PnAmW9mk-HCyjzhV_nxIoXpvQsFrtS1x8bVxR6rNVU9ORVD4E-ndtkI3VWq2P6p5QO842KZNBHbVbq4G8vZZ3oxEqmX6Cz29J4i44obPwbC8OVErY3aemTQGACYgD3_dUBsY57iF3xCnfV3qX-5zCNzT2pqC7xRbIxR8irg4jNKAfWLTmYwY6Fq_4iwc_v6iiVw4dE2xSrTjq9JQY5aTPYag7lufi0_GPyamjqJ2BFdwvby9AxPA4pz_SQAjB-2ArS7tOUSk7LI9pDTRLoGWV_PRa3H1b-TBHQVG4BWBJGrXaGJe96oTsbiA0qdmTEEPksIMLes_XUVtfl_Cfh1V3_yfEhIkel3y-jTLs-x5nGk4rJ6Nul0zWywiLEk8kKr5_JD_xyI4vDnhyh8X9x3clBeBM030_ea7h0Eyyd1GXbbOCXjln4nRKQ7VrHsJ1Ahl0ZmDPJozMVrJ_9-w65MGQgNpsNUQ7Kx52gU3Z4I2sQoG_1QjZIWHiFMqC8pGZThD53w5srpCADUkjn90haTScAM02jNK_2uDv31r3yMxOXJ0JrTodsAEJgYn-5DSKrfHZyjFNOVMh3Dtlo56PuGnsTIQpd1puDkZlHhno5t7nEaw0JsRbA84m98W7zDCWnq9jAUlXZKQG4). One of the things the authors did was vary floodplain characteristics in Delft3D to see if the style of avulsion changed, and one of the floodplain characteristics thewy changed was the critical shear stress. As I looked into their methods, I realized that because they used cohesive sediment on their floodplain, they could directly change the user-defined $/tau_c$, but because our floodplain is noncohesive (gravel and sand, no clay) we can only indirectly change the $/tau_c$ by changing the sediment size. Specifically, in Wilcock-Crowe formulation, the reference shear stress represents the critical shear stress, and is dependent on the grain sizes in the bed. A smaller median grain size would not only make particles easier to move, but making the distribution sandier might make gravel easier to move (up to a point) due to the hiding function in Wilcock-Crowe. 

I decided to test five different floodplain grain size distribution: 0.5, 0.75, 1, 1.25, 1.5 "fining factor" applied to the original measured floodplain $D_{50}$. This resulted in the following grain size distributions applied uniformly to all cells designated as erodible. 

<img width="913" alt="image" src="https://github.com/user-attachments/assets/aec60068-4045-4c87-acbd-d1466c74190a" />

*Figure 6. Raw and Normalized Floodplain Grain Size Distributions.*

I then built the sediment thickness files based on the four new distributions (not including the fining factor = 1, which is just the original floodplain distribution). 







