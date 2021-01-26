

## Solar Simulation & Shadow Analysis
Based on the ladybug sunpath the solar and shadow envelope are calculated in one file, since they have a largely corresponding step. To do so a cast a ray from the centroid of the voxel towards all the points on the sunpath. If a ray is not intersected by the context, then this voxel receives sunlight from this point. If the ray is intersected by the context, then the voxel does not receive sunlight from this point. For all the voxel that have been hit, the rays that were shot towards the sun are reversed, to calculate the shadow. If this ray intersects the context, then the voxel casts a shadow. If the ray does not intersect the context, then the voxel does not cast a shadow. Both the sunlight and the shadow envelope are then interpolated to a highres value, being of our voxel size. 

<center> <img src="https://cdn.discordapp.com/attachments/775754717346791494/803300922296893470/sun_and_shadow_hoofdstuk_3_sun_and_shadow.jpg"></center>

<center> <img src="https://cdn.discordapp.com/attachments/775754717346791494/801507207664762950/sun.png"></center>
*Sun calculation*

<center> <img src="https://cdn.discordapp.com/attachments/775754717346791494/801507211003822190/shadow.png"></center>
*Shadow calculation*


### Lowres size decisions
In the first run of the interpolated shadow file,  a lowres envelope of 2 voxels high was used. This resulted in the shadow casting calculation becoming much to generalized. In this situation it would see the entire bottom half of the building as not casting shadow on the neighbouring buildings, thus not showing them in the shadow casting and only showing the top half of the building in the visualisation. To solve this problem a lowres envelope of 3 voxels high was used. This resulted in a visualisation of the entire envelope. 

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/midterm/lowreshighres.png?raw=true">
________________________________________________
## Skylight & Skylight blocking
This script is very similar to the Solar simulation, but instead of loading a sunpath, a sphere is created to represent points in the sky. For each voxel a ray is cast from the centroid of the voxel towards all the points in the sky.  If a ray is not intersected by the context, then this voxel receives skylight from this point. If the ray is intersected by the context, then the voxel does not receive skylight from this point. For all the voxel that have been hit, the rays that were shot towards the sky are reversed, to calculate the skylight blocking. If this ray intersects the context, then the voxel casts blocks skylight from the context. If the ray does not intersect the context, then the voxel does not block skylight from the context. Both the skylight and the skylight blocking envelope are then interpolated to a highres value, being of our voxel size.

<img src="https://cdn.discordapp.com/attachments/775754717346791494/803300915749716088/skylight_and_skylight_blocking_hoofdstuk_3_skylight_and_skylight_blocking.jpg">

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/2.0_skyaccess.png?raw=true">

*skylight access*


<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/2.0_skyview.png?raw=true">

*skylight blocking*

### difference between shadow and solar
The result of the shadow envelope, solar envelope, skylight envelope and skylight blocking envelope  are processed in 2 ways. Fundamentally our building should be of least disturbance for the surrounding area, so it would not make sense to keep voxels that cast too much shadow or block too much skylight. Therefore, the voxels that cast too much shadow and block too much skylight from the context are removed. 
For the sun and skylight values a similar argument could be made. For sake of the building one could argue that only the voxels with the best sun and skylight access should be used. However, this would result in removing the voxels on the ground floor. Since building can’t float in the sky(yet), and for the accessibility of public functions its best to have them on the ground floor, the data of sunlight and skylight is stored inside the voxels for the growth model (link to growth model). 
 
###  Sun and Skylight improvements
Although these scripts are functional, there are still some improvements that could be made. 
    -	Factoring the influence of voxels inside the envelope on the sun/skylight and shadow/ skylight blocking. 
        o	For the initial stage of storing data and removing voxels, solely using the influence of the context on the voxels to be blocking sun or skylight is sufficient. But for later stages when the building is generated (link to growth model), it would be an improvement to take the influence of light and shadow of voxels on each other. This would make that script even heavier, because it will have to calculate a light and shadow value each time it adds voxels. Besides that, a distinction should made with the outer voxels and inner voxels, to have some depth in the building. This depth should also be specified. To solve this, the growth model could be normally ran at first, and then after it has finished going through an evaluation loop to check the shadow and skylight blocking on the context.
    -	Removing bad voxels based on the shadow the context receives instead of the percentage of the time voxels cause shadow.  
        o	As of now a threshold is specified to remove voxels from the envelope based on the percentage of time these voxels cast shadow and block skylight from the context. However, it would be an improvement if the voxels are removed based on the effect they have on the context. If a voxel casts a shadow on the context 50% of the time, this could mean that it causes it on a different building each time, meaning that although the voxel looks “bad”, the net result on the context is negligible. This calculation should also be taken into the growth model (link to growth model). This does mean this script will become even more heavy as it now must calculate the shadow and skylight blocking each time it grows as well. To solve this, the growth model could be normally ran at first, and then after it has finished going through an evaluation loop to check the shadow and skylight blocking on the context. 


## Quietness
To calculate the quietness noise in the building in relation to its context a path with noise points on it is loaded inside a script. Each voxel then calculates the distance from the voxel centroid towards the noise point. These distances are added together and converted into a ratio. This script is actually more about business on street level than about quietness, since it imports noise points and not actual decibels from areas. This does not matter too much, since noise could be filtered from a building by adding more insulation and the relative quietness matters more than the actual numbers in decibels

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/3.2_quietness.png?raw=true" style="width:280px;">

*Noise highres(quietness)*

________________________________________________

## Entrances and distance lattices to these entrances
For this script, a design decision is needed in regards to the placement of the entrances. For this, the following site analysis has been made: 

<img src="https://cdn.discordapp.com/attachments/784009094474366977/803248102412779550/entrances.jpg">

As is shown, the accessibility from the city centre is highest on the south and westside of the plot. The main connection with the city centre is the route from the south, via the Luchtsingel and the Hofbogen. To expand the atmosphere of this vivid area, the public functions should have short distance to this place. By placing them across the old metroline along the westside of the plot, the street ambiance will get a new boost, the old metroline can be made in good use and an extension of the vivid street scenery from the city centre is then generated. 
To make this work, a distance lattice is being made. the distance is calculated from all streetlevel voxels that are along this street. This would attract all functions that care about having a public entrance; fablabs, café pub restaurant, arcade, shop, co-working spaces and startup office. By not giving them a determined entrance, there stays room for these functions to not only grow towards this set location but it gives them a range of places where they can grow with consideration of other values.  
As the accessibility from the city centre is also high for the eastside of the building, this could function as a more private, yet highly used entrance side. All communal functions could be located here, as well as the main housing entrances and the gym. For these functions, a few different lattices are generated, as the housing access needs a number of set location on different sides of the building, and the community centre entrance is set next to this housing entrance to have a entrance that brings people together.
For the parking entrance, the location that has been chosen is close to the housing entrance, yet it should minimize car traffic in the neighbourhood and is therefore determined on the very east side of the building, close to bigger streets. 
For each lattice, a distance graph is generated for all voxels within the envelope. To minimize these calculations it is done over a bigger voxelsize. After generating this graph, the specific entrance voxels are selected, and with these voxels of interest a distance lattice is generated. After normalizing, interpolation takes place to translate all values to the actual voxelsize that is being used. 


<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/3.3_lattice_public_entrances.png?raw=true">

*distance lattice public entrance*

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/3.4_lattice_gym.png?raw=true">

*distance lattice gym*

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/3.5_lattice_parking_entrance.png?raw=true">

*distance lattice parking entrance*

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/3.6_lattice_housing.png?raw=true">

*distance lattice housing entrances*

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/3.7_lattice_comcen_entrance.png?raw=true">

*distance lattice community centre*

### Improvements 
To make an even more accurate and generative design, the initial locations of the entrances should not be chosen by hand but determined in a calculation of accessibility of the plot. This would be done by making an extensive model of all surrounding streets: their bustle, their usability for different types of traffic and their potential. This extensive model could not only be used for generating entrances, but also for a noise calculation to value all voxels based on the traffic. 
This more detailed way of locating an entrance should be implemented during the growth model, so closeness of other entrances would be taken into account for generating the wished for street ambiance

________________________________________________
