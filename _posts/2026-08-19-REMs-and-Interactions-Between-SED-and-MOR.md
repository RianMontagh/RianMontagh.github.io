# REMs and Interactions between the SED and MOR Files

## Continuing to work on my REMs

Last week I left of after creating a centerline based on flow accumulation. Next, I started translating the methods used in this [Python notebook](https://docs.hyriver.io/examples/notebooks/rem.html) to MATLAB. To start, I wrote a script to make the initial Delft3D unstructured grid into an REM. Then I can use the product from this analysis to compare all of the final bed elevations to. 

**Step 1.** Sample river bed elevation along the centerline. 

This step involved some smoothing of the original centerline, which reduced some of the variability in elevation. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/b05e9103-0896-42fd-ab69-3d64600beed5" />

*Figure 1. Longitudinal Bed Elevation*

**Step 2.** Perform Inverse Weighted Differencing (IDW) across the floodplain to create our reference elevation that will be subtracted from the actual elevation. 

In IDW, you interpolate by finding the k nearest neighbors of your sample points to your query point. In this case, the sample points are the river bed elevation along the centerline, and the query points are the grid cells. In ESRI's words:

>In IDW one uses the measured values surrounding the prediction location to predict a value for any unsampled location, based on the assumption that things that are close to one another are more alike than those that are farther apart.

The equation used is:

<img width="491" alt="image" src="https://github.com/user-attachments/assets/35351837-9e9a-486d-bd7b-48bb941346d6" /> (wikipedia)

x: an interpolated (arbitrary) point  
x_i: is an interpolating (known) point  
d: a given distance (metric operator) from the known point xi to the unknown point x  
N: the total number of known points used in interpolation  
p: a positive real number, called the power parameter  

I used the [`KDTreeSearcher`](https://www.mathworks.com/help/releases/R2025b/stats/kdtreesearcher.html?searchPort=59737) and [`knnsearch`](https://www.mathworks.com/help/releases/R2025b/stats/exhaustivesearcher.knnsearch.html) functions to find the nearest neighbors. `KDTreeSearcher` saves the results of a nearest neighbor search on my known data (elevation along the centerline) with a Kd-tree algorithm. `knnsearch` then returns the indices of the N nearest neighbors to the query point and the distances from the query point to the N number of neighbors. Now I have all the components of the equation above. 

I took `N=300` and `p=2`. I noticed that decreasing N caused the floodplain to get a choppier look, which makes sense as the interpolation relies on few, more local points. As I increased `N`, I saw more smooth, realistic floodplains that did not affect the crispness of the channel and channel meanders. When I increased `p` I noticed that the results got the same choppy or streaky look as the low `N`. This also makes sense because as `p` increases, the more local or shorter distances `d` are weighted more, which means the results will not be as smooth. I chose `p=2` because that is the default `p` for two-dimensional IDW tool in ArcGIS Pro. 

>Note: The [documentation](https://pro.arcgis.com/en/pro-app/3.5/help/analysis/geostatistical-analyst/how-inverse-distance-weighted-interpolation-works.htm) for this ArcGIS tool states, "When p = 2, the method is known as
the inverse distance squared weighted interpolation. The default value is p = 2, although there is no theoretical justification to prefer this value over others, and the effect of changing p should be investigated by previewing the output and examining the cross-validation statistics."

N = 10                                                                                |  N = 300
:------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------:
![](https://github.com/user-attachments/assets/c4f969f8-a885-42d2-8afc-58a88edb4a94)  |  ![](https://github.com/user-attachments/assets/e0e4ab78-1593-417d-91ac-04e10a2fac5e)

p = 1                                                                                 |  p = 10
:------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------:
![](https://github.com/user-attachments/assets/36980e68-4086-4922-bee7-244fd1ce5e41)  |  ![](https://github.com/user-attachments/assets/49ac60da-5e50-49c8-a968-3df21c9c228d)

### Re-plotting Past Tests as REMs

I showed this figure last week and re-plotted it this week with my new method for REMs. I think that the braided section upstream is much clearer now as compared to the raw plot of bed elevation. 

<img width="1826" alt="image" src="https://github.com/user-attachments/assets/1b77903f-0133-42f1-a33b-6ce2c329d7ce" />

*Figure x. My past runs with the old model but as REMs*

<img width="1612" alt="image" src="https://github.com/user-attachments/assets/bf8aef4d-00f9-47e8-b85c-dc475359178b" />

*Figure x. My past runs with the old model before implementing the REM method*

## Interactions Between SED and MOR Files

Once I got my REM script to my liking, I refocused on exploring the causes and inhibitors of floodplain erosion in the new model. Specifically, I was curious if the MOR file was solely responsible for the channels seen when running the current model with the old SED file, old sediment fractions, and old MOR file. So, I ran the current model just with the old MOR file. Turns out that it does not explain everything. 

<img width="1826" alt="image" src="https://github.com/user-attachments/assets/2e1d0a22-5dc3-4d54-9d76-35471fb7f39d" />

*Figure x. Tests with the old model. See that the MOR file alone does not explain the behavior of the second plot.*















