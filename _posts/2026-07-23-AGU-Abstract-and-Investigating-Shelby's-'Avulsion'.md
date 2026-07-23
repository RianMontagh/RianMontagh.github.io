# AGU Abstract and Investigating Shelby's 'Avulsion'

## AGU Abstract

I wrote a first draft of my abstract for AGU. My overall thoughts are that I had the hardest time with writing a sentence about what we expect to find or what our hypothesis is. I ended up saying that I think the floodplain will be very important, because it has been in terms of turning on and off erodibility. But when I change the sediment sizes, I didn't see much of an effect on the amount of Q to Sumas. 

> A river avulsion is the natural process during which a river channel suddenly changes course. In the past several thousand years, the Nooksack River in northwest Washington State avulsed from a north-flowing channel to its present westward course. During large floods, the topographic low of the historic north-flowing Nooksack River valley becomes inundated, placing communities at risk of loss of life and causing substantial economic damage. This study uses a two-dimensional morphodynamic Delft3D-FM model forced with climate-projected future flood hydrographs to evaluate the mechanisms and likelihood of the Nooksack permanently avulsing back to its historic course on an event timescale. Model parameters such as floodplain roughness and sediment coarseness as well as flood peak magnitude, flood duration, and number of flood events were perturbed to investigate their effects on avulsion. We hypothesize that floodplain roughness and sediment sizes exert strong controls on the initiation of avulsion during extreme floods. Results from this study will improve understanding of event-scale avulsion processes for similar watersheds and inform floodplain management strategies aimed at reducing avulsion risk in the Nooksack River.

## Potential Sessions

- **[EP009](https://agu.confex.com/agu/agu26/prelim.cgi/Session/282227) - Cross-Scale Sediment Modeling and Prediction: Integrating Physics-Based, Data-Driven, and Hybrid Modeling**
- **[EP010](https://agu.confex.com/agu/agu26/prelim.cgi/Session/283485) - Earth and Planetary Surface Processes General Contributions**
    - Charles Shobe and Dan Scott’s paper is included as one of the 5 papers
- **[EP020](https://agu.confex.com/agu/agu26/prelim.cgi/Session/282961) - Land Surface Hazards Across Landscapes: Linking Triggers, Processes, and Consequences**
    - Brian Yanites (Brooke’s old advisor) is the primary convenor
- **[EP022](https://agu.confex.com/agu/agu26/prelim.cgi/Session/280520) - Morphodynamic Processes in Human-Impacted Fluvio-Lacustrine Environments**
    - probably not relevant because of the focus on human-impacted
- **[EP027](https://agu.confex.com/agu/agu26/prelim.cgi/Session/281088) - River and Floodplain Dynamics: Linking Geomorphic Processes in Pristine and Recovered Systems**
    - **This session invites contributions that advance understanding of river floodplain dynamics across natural and managed systems. Topics may include sediment and wood dynamics, floodplain evolution, system responses to restoration activities such as dam removal, levee setbacks, and channel remeandering, and linkages between physical recovery and ecological or biogeochemical function.**
 
## Rerunning the Old Model

I reran what I am calling 'old model' where Shelby observed incision of channels into the floodplain. I did this because the original run was extremely large, so I changed several of the settings to make the results more compressed and to make the `merge.slurm` script run faster. See below for the settings that are required to make the output size smaller and merge.slurm script faster:

MDU File

    MapFormat                         = 4                                                                   # Map file format (1: NetCDF, 2: Tecplot, 3: NetCFD and Tecplot)
    NcFormat                          = 4                                                                   # Format for all NetCDF output files (3: classic, 4:NetCDF4+HDF5).
    NcMapDataPrecision                = single                                                              # Precision for NetCDF data in map files (double or single).
    NcHisDataPrecision                = single                                                              # Precision for NetCDF data in his files (double or single).
    NcCompression                     = 1                                                                   # Whetherornot (1/0) to apply compression to NetCDF output files-NOTE:only works when NcFormat=4.
    
MOR File

    TrackMassShortage        = false
    
    [Output]
    Default               = false
    BedTranspAtFlux       = false
    BedTranspDueToCurrentsAtZeta= false
    BedTranspDueToCurrentsAtFlux= false
    RawTransportsAtZeta   = false
    SuspTranspAtFlux      = false
    EquilibriumConcentration= false
    NearBedRefConcentration= false
    SettlingVelocity      = false
    Bedforms              = false
    Dm                    = false
    Dg                    = false
    Frac                  = false
    MudFrac               = false
    BedLayerSedimentMass  = false
    BedLayerDepth         = false
    BedLayerVolumeFractions= false
    BedLayerPorosity      = false
    TranspType            = 1

My merge.slurm script worked well and I got results for the entire model run.

Here is my elevation plot at the end of the old model now.

<img width="803" alt="image" src="https://github.com/user-attachments/assets/a6a755c7-e5e9-457d-822c-994adfcaabb2" />


**Next steps**
1. Make comparison plots between the old model sediment fraction and current sediment fraction (should be the same on the floodplain).
2. Make elevation plots for my climate-projected floods to see if they also have incised channels.
3. Connect with Shelby next week.

## Fluvial Geomorphology Class with Allison

I messaged Allison about taking her class at WWU, and she said it would be possible. She teaches in the spring and has some old recordings from the pandemic that I could watch. However, she thinks it would be good if I come up on Wednesdays for their field trips. The only problem with this is that I believe sediment transport is being taught in the spring as well. I might have to choose between the two classes depending on scheduling. 
