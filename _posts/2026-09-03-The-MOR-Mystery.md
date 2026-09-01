# The MOR Mystery 

## Thoughts on Updated Overflow to Sumas Plot

I updated my overflow to Sumas plot form last week with a few runs that had to be redone. The `Old SED file + old sed frac + ThetSD 0` needed to be reran with the correct hydrograph and the `SED file + old sed frac + ThetSD 0` and `Old SED file + old sed frac + SedThr 0.1` needed to be run with a longer runtime limit in the `run_202601.sh` file. Rerunning the `Old SED file + old sed frac + SedThr 0.1` fixed the low overflow values that were shown in the plot last week. I am not sure why that is, but it seems like there is some process that was cut off at the end of the run due to the 24 hour runtime limit that messed up the results of the model. I also reordered the plotting order to be from highest to lowest overflow discharge to make the plot more legible. The zoomed-in plot is still much easier to read. 

<img width="1410" alt="image" src="https://github.com/user-attachments/assets/319055cd-901c-4abc-baf8-1ba485818925" />

*Figure 1. Corrected Plot from last week*

<img width="1390" alt="image" src="https://github.com/user-attachments/assets/909a870f-6493-4514-bdee-3c5bec12747c" />

*Figure 2. Zoomed-in corrected Plot from last week*

There are a few surprising observations from these results.

1. As noted last week, there is still a large-ish gap between the `Old SED file + old sed frac + Old MOR` run and the `Old SED file + old sed frac + Old MOR` run. The only difference between these two models are a few other MOR settings, which is why I called this blog "The MOR Mystery". During my study of the MOR file, SedThr and ThetSD seemed to be the only settings capable of making a difference in the floodplain erodibility, but these results indicate there is more going on. The other settings I have not tested yet are:

