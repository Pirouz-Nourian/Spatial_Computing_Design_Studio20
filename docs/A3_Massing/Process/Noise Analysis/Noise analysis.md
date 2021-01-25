# Noise analysis

## Exterior noise

Because our plot is located in the centre of a large city (Rotterdam) we need to be able to cope with noise. First of all we need to calculate the exterior noise to which our building will be exposed to. For this we, in the urban analysis, analyzed a map of the "Atlas van de leefomgeving". In here we saw that there is a busy road (Heer Bokelweg) within reach of 100 meters of the plot. The noise level of this road was 70 decibel. To represent this road in our notebook, we have placed several noise points at the location of the street and gave them a noise base level of 70. Thereafter we calculated the Euclidean distance from the road to the plot. TO calculate the noise lattice we used a formula of the book *Environmental Noise* by ... Because a road is a line source we used this formula: 

Lp = LW - 10 log10 (r) – 5 dB

## Noise lattice 

![title](../../../img/noise_field.png)

## Pseudo code

``` python

Input: Final envelope (high res)

1. Import envelope lattice

2. Load noise source points 

3. Creation of Noise field
Extract the coordinates of the centroids of the voxels
Set a noise base level for the noise points
Calculate the Euclidean distance between the centroids and the noise points 
Compute the noise lattice with the noise base level and the Euclidean distance 

Output: Noise field lattice


```

[Noise field full notebook](/spatial_computing_project_template/index/scripts/noise_field/)

## Interior noise


