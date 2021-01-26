# Spatial analysis: generating all voxel values
To be able to locate all functions at their optimal location in the lattice, value lattices are to be generated. This to determine which voxels are where  each voxel of the lattice needs values for:
1.	Skyview
2.	Sunlight availability
3.	Quietness 
4.	Closeness to an entrance
To generate these values, different computations are being made, and are being explained on this page. For each computation, the calculations are first being executed over the low-resolution lattice, after which they are interpolated to the final lattice with the voxel size of 1.8m * 1.8m * 1.8m. 

## Sun and skylight Analysis

Based on the ladybug sunpath the solar envelope is calculated. To do so a ray is cast from all the centroids of the voxels towards all the points on the sunpath. If a ray is not intersected by the context, then this voxel receives sunlight from this point. If the ray is intersected by the context, then the voxel does not receive sunlight from this point. This  envelope is then interpolated to a highres value. 

For Skylight all these steps are repeated, but instead of loading a sunpath, a sphere is created to represent the sky. Instead of shooting rays towards the sunpoints, the rays are being shot to the skypoints. 
<center> <img src="https://cdn.discordapp.com/attachments/785803868356476958/803590070753427486/sun_and_skylight.jpg> </center>


For the sun and skylight values a similar argument could be made as for the shadow and skylight blocking. For sake of the building one could argue that only the voxels with the best sun and skylight access should be used. However, this would result in removing the voxels on the ground floor. Since building can’t float in the sky (yet), and for the accessibility of public functions its best to have them on the ground floor, the data of sunlight and skylight is stored inside the voxels for the growth model (link to growth model).



<center> <img src="https://media.discordapp.net/attachments/775754717346791494/801507207664762950/sun.png?width=901&height=676">

*Sun availability calculation*</center>


<center><img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/2.0_skyaccess.png?raw=true">

*skylight availability calculation* </center>

 
###  Sun and Skylight improvements
Although these scripts are functional, there are still some improvements that could be made. 
    -	Factoring the influence of voxels inside the envelope on the sun/skylight and shadow/ skylight blocking. 
        o	For the initial stage of storing data and removing voxels, solely using the influence of the context on the voxels to be blocking sun or skylight is sufficient. But for later stages when the building is generated (link to growth model), it would be an improvement to take the influence of light and shadow of voxels on each other. This would make that script even heavier, because it will have to calculate a light and shadow value each time it adds voxels. Besides that, a distinction should made with the outer voxels and inner voxels, to have some depth in the building. This depth should also be specified. To solve this, the growth model could be normally ran at first, and then after it has finished going through an evaluation loop to check the shadow and skylight blocking on the context.
    -	Removing bad voxels based on the shadow the context receives instead of the percentage of the time voxels cause shadow.  
        o	As of now a threshold is specified to remove voxels from the envelope based on the percentage of time these voxels cast shadow and block skylight from the context. However, it would be an improvement if the voxels are removed based on the effect they have on the context. If a voxel casts a shadow on the context 50% of the time, this could mean that it causes it on a different building each time, meaning that although the voxel looks “bad”, the net result on the context is negligible. This calculation should also be taken into the growth model (link to growth model). This does mean this script will become even more heavy as it now must calculate the shadow and skylight blocking each time it grows as well. To solve this, the growth model could be normally ran at first, and then after it has finished going through an evaluation loop to check the shadow and skylight blocking on the context. 


## Quietness
To calculate the quietness noise in the building in relation to its context a path with noise points on it is loaded inside a script. Each voxel then calculates the distance from the voxel centroid towards the noise point. These distances are added together and converted into a ratio. This script is actually more about business on street level than about quietness, since it imports noise points and not actual decibels from areas. This does not matter too much, since noise could be filtered from a building by adding more insulation and the relative quietness matters more than the actual numbers in decibels

<center><img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/3.2_quietness.png?raw=true">

*Noise highres(quietness)*</center>

________________________________________________

## Entrances and distance lattices to these entrances
For this script, a design decision is needed in regards to the placement of the entrances. For this, the following site analysis has been made: 

<center><img src="https://cdn.discordapp.com/attachments/784009094474366977/803248102412779550/entrances.jpg"></center>

As is shown, the accessibility from the city centre is highest on the south and westside of the plot. The main connection with the city centre is the route from the south, via the Luchtsingel and the Hofbogen. To expand the atmosphere of this vivid area, the public functions should have short distance to this place. By placing them across the old metroline along the westside of the plot, the street ambiance will get a new boost, the old metroline can be made in good use and an extension of the vivid street scenery from the city centre is then generated. 
To make this work, a distance lattice is being made. the distance is calculated from all streetlevel voxels that are along this street. This would attract all functions that care about having a public entrance; fablabs, café pub restaurant, arcade, shop, co-working spaces and startup office. By not giving them a determined entrance, there stays room for these functions to not only grow towards this set location but it gives them a range of places where they can grow with consideration of other values.  
As the accessibility from the city centre is also high for the eastside of the building, this could function as a more private, yet highly used entrance side. All communal functions could be located here, as well as the main housing entrances and the gym. For these functions, a few different lattices are generated, as the housing access needs a number of set location on different sides of the building, and the community centre entrance is set next to this housing entrance to have a entrance that brings people together.
For the parking entrance, the location that has been chosen is close to the housing entrance, yet it should minimize car traffic in the neighbourhood and is therefore determined on the very east side of the building, close to bigger streets. 
For each lattice, a distance graph is generated for all voxels within the envelope. To minimize these calculations it is done over a bigger voxelsize. After generating this graph, the specific entrance voxels are selected, and with these voxels of interest a distance lattice is generated. After normalizing, interpolation takes place to translate all values to the actual voxelsize that is being used. 

<center>
<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/3.3_lattice_public_entrances.png?raw=true">

*distance lattice public entrance*

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/3.4_lattice_gym.png?raw=true">

*distance lattice gym*

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/3.5_lattice_parking_entrance.png?raw=true">

*distance lattice parking entrance*

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/3.6_lattice_housing.png?raw=true">

*distance lattice housing entrances*

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/3.7_lattice_comcen_entrance.png?raw=true">

*distance lattice community centre*</center>

### Improvements 
To make an even more accurate and generative design, the initial locations of the entrances should not be chosen by hand but determined in a calculation of accessibility of the plot. This would be done by making an extensive model of all surrounding streets: their bustle, their usability for different types of traffic and their potential. This extensive model could not only be used for generating entrances, but also for a noise calculation to value all voxels based on the traffic. 
This more detailed way of locating an entrance should be implemented during the growth model, so closeness of other entrances would be taken into account for generating the wished for street ambiance

