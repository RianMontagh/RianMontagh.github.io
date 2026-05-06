# Adjusting Erodibility and Creating Sequential Flood Hydrographs

This week my goal is to 

1. Remove the bridges from the non-erodible areas and rerun the "nonerodible" models with the updated sediment thickness files. 
2. Run morpho off models for all durations.
3. Start thinking about a more realistic way to adjust floodplain erodibility.
4. Work on a more comprehensive plot of bed change vs. river station.
5. Look into changing the shape of the 100-year storm based on the Deming 100-year hydrograph from Whatcom County.
6. Start EFM presentation.

# Removing the Bridges

With some ArcGIS Pro work, I was able to edit the pixels in the original NLCD raster to reclassify the bridges (Developed land use type) to the Open Water designation. Then I reran my script that adds the nonerodible areas to the sediment fractions, essentially making those areas have 0 meters of erodible material (sediment thickness). While I was moving through the NLCD raster in ArcGIS Pro, I noticed that a future modification would be to have the Developed, Open Space category to erodible. It seems like a lot of this land is just open fields that are nearby development. Since they do not have pavement or hard built structures, this should probably be nonerodible. The first two figures in this blog show the before and after of the bridge removal. Notice that for the Everson corridor, Everson Bridge used to be represented as non-erodible. 

<img width="1396" alt="image" src="https://github.com/user-attachments/assets/a4bdb46b-e375-40f8-be75-82d33be7449c" />

*Figure 1. Sediment Fractions at the Everson Corridor with Non-Erodible Areas at the Bridges*

<img width="1392" alt="image" src="https://github.com/user-attachments/assets/6cc094df-b9f0-4b66-822f-76e7e5dec527" />

*Figure 2. Sediment Fractions at the Everson Corridor with Non-Erodible Areas Removed at the Bridges*

