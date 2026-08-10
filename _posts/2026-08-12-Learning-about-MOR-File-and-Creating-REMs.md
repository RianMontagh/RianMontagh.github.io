# Learning about MOR File and Creating REMs

## Additional Test with the Current Model

<img width="1612" alt="image" src="https://github.com/user-attachments/assets/09b80b77-6bd3-4dbe-be12-77407b4d76dd" />

<img width="1620" alt="image" src="https://github.com/user-attachments/assets/78646005-75e1-4e63-8863-18fa2748bc0e" />

## Additional Test with the Old Model 

<img width="1612" alt="image" src="https://github.com/user-attachments/assets/bf8aef4d-00f9-47e8-b85c-dc475359178b" />

## Examining the MOR File

I started combing through the MOR file to see what changes have been made between the old and current model. I made a table for each different section of settings, see below. 

| MOR Morphology Setting | Old Model | Current Model | Description |
| ----------- | --------- | ------------- | ----------- |
| SedThr | 0.1 | 2 |
| DensIn | 0 | 1 |
| NeglectEntrainment | -- | false |
| NeuBcSand | -- | true |
| ISlope | 2 | 3 |
| IHidExp | 1 | 3|
| SusW | 1 | 0 | 
| BedW | 1 | 0 |
| ThetSD | 0 | 0.5 | 
| HMaxTH | 1.5 | 1 |
| UpdInf | -- | false |
| DzMax | -- | 0.05 | 

| MOR Underlayer Setting | Old Model | Current Model | Description |
| ----------- | --------- | ------------- | ----------- |
| MxNULyr | 50 | 100 |
| TTLForm | 1 | 2 |
| ThTrLyr | 0.2 | 0.05 | 
| TTLAlpha | -- | 0.05 |
| TTLMin | -- | 0 |
| ThUnLyr | 0.256 | 0.2 | 

| MOR Numerics Setting | Old Model | Current Model | Description |
| ----------- | --------- | ------------- | ----------- |
| UpwindBedload | -- | true | 
| LaterallyAveragedBedload | -- | false | 
| MaximumWaterdepth | -- | true | 
| MaximumWaterdepthFraction | -- | 1 | 

> The Output settings section also had several differences originally, but in the runs I am analyzing they are the same in order to make the output compatible with the merge.slurm script. This script merges the map partitions into one NetCDF file on Hyak. To my understanding, these settings only affect what is being saved, so they shouldn't change how the model is working. 




