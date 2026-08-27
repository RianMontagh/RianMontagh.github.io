# Isolating Sediment Transport and Morphology Variables

## Testing the Effects of Acal, SedThr, and ThetSD

This week, after seeing that a combination of sediment transport and morphology settings are responsible for the incised channels we see in the older version of the model, I ran more specific combinations of model settings to drill down on the role that each setting plays. 

The models I ran are the current model:
- Old SED + sed frac + SedThr 0.1
- Old SED + sed frac + SedThr 0.1 + ThetSD 0
- Old SED + sed frac + ThetSD 0
- Acal 4 + SedThr 0.1

Interestingly, the `Acal 4 + SedThr 0.1` and `Old SED + sed frac + SedThr 0.1` runs both exceeded their runtime limits of 24 hours. I investigated why this might be and reran the models with a new runtime limit of four days. 

### Bed Change Plots

<img width="1546" alt="image" src="https://github.com/user-attachments/assets/5edb1f5f-4013-482a-a5db-96d0576308b3" />

*Figure 1. Bed change for old sediment and morphology settings*

<img width="1546" alt="image" src="https://github.com/user-attachments/assets/6d1c6d90-57fb-462e-b686-079e3173b963" />

*Figure 2. Bed change testing ThetSD setting*

<img width="1546" alt="image" src="https://github.com/user-attachments/assets/cb174928-b0ea-4c23-bac3-041ec5ca9beb" />

*Figure 3. Bed change testing ThetSD and SedThr*

### Relative Elevation Plots

<img width="1543" alt="image" src="https://github.com/user-attachments/assets/fa808e62-ebb0-454a-9ca1-22d4a73927da" />

*Figure 4. Relative elevation for old sediment and morphology settings*

<img width="1543" alt="image" src="https://github.com/user-attachments/assets/edbb43a7-b41d-4946-ad4c-e8e4760165a2" />

*Figure 5. Relative elevation testing ThetSD setting*

<img width="1543" alt="image" src="https://github.com/user-attachments/assets/a09a761c-bda6-4711-9435-f2ac2a9b57fd" />

*Figure 6. Relative elevation testing ThetSD and SedThr*

### Google Sheet Tracker for my 'Current model' tests

<iframe
  src="https://docs.google.com/spreadsheets/d/e/2PACX-1vTNEERp8HfxdkPjvOmIc69DhdCgLg4YO1hSy51zQ8ePTpRpJDvYmwvY807dcBRW7eRr8sL-Y5s3NjnH/pubhtml?gid=0&amp;single=true&amp;widget=true&amp;headers=false"
  width="100%"
  height="800"
  frameborder="0">
</iframe>

## Investigating Slow Runtimes with SedThr = 0.1

<img width="1356" height="907" alt="image" src="https://github.com/user-attachments/assets/c9947012-ae8b-403e-a3ca-aeea02a10c66" />
<img width="1388" height="907" alt="image" src="https://github.com/user-attachments/assets/3694d0ff-7225-4e1b-afe9-4f13871cfcf4" />


