<img src="/img/logo/apidae_black.png" alt="collective" style="width:280px;"> 
<center> <img src="/img/logo/apidae_with_text.png" alt="collective" style="width:600px;"> </center>

<img src="/img/midterm/lowreshighres.png" style="width:280px;">

1<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/0.0_basecontext.png?raw=true" style="width:280px;"></img>

2<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/0.0_mesh.png?raw=true" style="width:280px;">

3<img src="/img/finalscreenshots/0.1_fulllattice.png" style="width:280px;">


4<img src="/img/finalscreenshots/2.0_skyaccess.png" style="width:280px;"></img>


5<img src="/img/finalscreenshots/2.0_skyview.png" style="width:280px;">


6<img src="/img/finalscreenshots/2.1.0_selection.png" style="width:280px;">


7<img src="/img/finalscreenshots/2.1.1_removed.png" style="width:280px;">


8<img src="/img/finalscreenshots/2.2_newfulllattice.png" style="width:280px;"></img>


9<img src="/img/finalscreenshots/3.1.0_sunaccess.png" style="width:280px;"></img>


10<img src="/img/finalscreenshots/3.1.2_shadow.png" style="width:280px;"></img>


11<img src="/img/finalscreenshots/3.2_quietness.png" style="width:280px;"></img>


12<img src="/img/finalscreenshots/3.3_lattice_public_entrances.png" style="width:280px;"></img>

13<img src="/img/finalscreenshots/3.4_lattice_gym.png" style="width:280px;">

14<img src="/img/finalscreenshots/3.5_lattice_parking_entrance.png" style="width:280px;">

15<img src="/img/finalscreenshots/3.6_lattice_housing.png" style="width:280px;">

16<img src="/img/finalscreenshots/3.7_lattice_comcen_entrance.png" style="width:280px;">

17<img src="/img/finalscreenshots/4.1_average_voxel_value.png" style="width:280px;">

18<img src="/img/finalscreenshots/4.1_averagevoxelval.png" style="width:280px;">

19<img src="/img/finalscreenshots/4.2_new_avail_lattice.png" style="width:280px;">

20<img src="/img/finalscreenshots/5.1_corridors_groundfloor.png" style="width:280px;">

21<img src="/img/finalscreenshots/6.1_corridors_groundfloor.png" style="width:280px;">

22<img src="/img/finalscreenshots/6.1_corridors_upper_floors.png" style="width:280px;">

23<img src="/img/finalscreenshots/6.2_lattice_distance_communal_functions.png" style="width:280px;">

24<img src="/img/finalscreenshots/6.2_lattice_distance_upper_floors.png" style="width:280px;">

25<img src="/img/finalscreenshots/6.3_shafts.png" style="width:280px;">

26<img src="/img/finalscreenshots/6.4_all_shafts_and_corridors.png" style="width:280px;">

27<img src="/img/finalscreenshots/6.4_shafts_and_corridors.png" style="width:280px;">

28<img src="/img/finalscreenshots/6.4_shafts_and_corridors_in_boudningbox.png" style="width:280px;">

29<img src="/img/finalscreenshots/7.0_initialization.png" style="width:280px;">

30<img src="/img/finalscreenshots/7.0_initialization_with_lattice.png" style="width:280px;">

31<img src="/img/finalscreenshots/7.0_time0.png" style="width:280px;">

32<img src="/img/finalscreenshots/7.0_time100.png" style="width:280px;">

33<img src="/img/finalscreenshots/7.0_time500.png" style="width:280px;">

34<img src="/img/finalscreenshots/7.0_time1000.png" style="width:280px;">

35<img src="/img/finalscreenshots/7.0_time2500.png" style="width:280px;">

36<img src="/img/midterm/trap.jpg" style="width:280px;">
37<img src="/img/midterm/TRAP_2.jpg" style="width:280px;">



# Process of massing

## Solar Simulation & Shadow Analysis
Based on the ladybug sunpath the solar and shadow envelope are calculated in one file, since they have a largely corresponding steps. To do so a cast a ray from the centroid of the voxel towards all the points on the sunpath. If a ray is not intersected by the context, then this voxel receives sunlight from this point. If the ray is intersected by the context, then the voxel does not receive sunlight from this point. For all the voxel that have been hit, the rays that were shot towards the sun are reversed, to calculate the shadow. If this ray intersects the context, then the voxel casts a shadow. If the ray does not intersect the context, then the voxel does not cast a shadow. Both the sunlight and the shadow envelope are then interpolated to a highres value, being of our voxel size.  

### Lowres size decisions
In the first run of the interpolated shadow file,  a low res envelope of 2 voxels high was used. This resulted in the shadow casting calculation becoming much to generalized. In this situation it would see the entire bottom half of the building as not casting shadow on the neighbouring buildings, thus not showing them in the shadow casting and only showing the top half of the building in the visualisation. To solve this problem a lowres envelope of 3 voxels high was used. This resulted in a visualisation of the entire envelope.  

<img src="/img/midterm/lowreshighres.png" style="width:280px;">

## Skylight & Skylight blocking
This script is very similar to the Solar simulation, but instead of loading a sunpath, a sphere is created to represent points in the sky. For each voxel a ray is cast from the centroid of the voxel towards all the points in the sky.  If a ray is not intersected by the context, then this voxel receives skylight from this point. If the ray is intersected by the context, then the voxel does not receive skylight from this point. For all the voxel that have been hit, the rays that were shot towards the sky are reversed, to calculate the skylight blocking. If this ray intersects the context, then the voxel casts blocks skylight from the context. If the ray does not intersect the context, then the voxel does not block skylight from the context. Both the skylight and the skylight blocking envelope are then interpolated to a highres value, being of our voxel size.


### difference between shadow and solar
The result of the shadow envelope, solar envelope, skylight envelope and skylight blocking envelope  are processed in 2 ways. Fundamentally we want our building to be of least disturbance for the surrounding area, so it would not make sense to keep voxels that cast too much shadow or block too much skylight. This is why we remove the voxels that cast too much shadow and block too much skylight from the context. 
For the sun and skylight values a similar argument could be made. For sake of the building one could argue that only the voxels with the best sun and skylight access should be used. However, this would result in removing the voxels on the ground floor. Since building can’t float in the sky(yet), and for the accessibility of public functions its best to have them on the ground floor, the data of sunlight and skylight is stored inside the voxels for the growth model (link to growth model). 


### Removing voxels
To not cause too much shadow or block too much skylight from the context, the voxels that cast too much shadow or block too much skylight from the context, are removed from the envelope based on a threshold. This results in a new envelope without the “bad voxels” 

<iframe src="https://thumbs.gfycat.com/ValidImaginativeChihuahua-size_restricted.gif" style="width:150%; height:430px;" frameborder="0"></iframe>

<img src="/img/finalscreenshots/2.2_newfulllattice.png" style="width:280px;">


##  Sun and Skylight improvements
Although these scripts are functional, there are still some improvements that could be made. 
    -	Taking into account the influence of voxels inside the envelope on the sun/skylight and shadow/ skylight blocking. 
        o	For the initial stage of storing data and removing voxels, solely using the influence of the context on the voxels to be blocking sun or skylight is sufficient. But for later stages when the building is generated (link to growth model), it would be an improvement to take the influence of light and shadow of voxels on each other. This would make that script even heavier, because it will have to calculate a light and shadow value each time it adds voxels. Besides that, a distinction should made with the outer voxels and inner voxels, to have some depth in the building. This depth should also be specified. To solve this, the growth model could be normally ran at first, and then after it has finished going through an evaluation loop to check the shadow and skylight blocking on the context.
    -	Removing bad voxels based on the shadow the context receives instead of the percentage of the time voxels cause shadow.  
        o	As of now a threshold is specified to remove voxels from the envelope based on the percentage of time these voxels cast shadow and block skylight from the context. However it would be an improvement if the voxels are removed based on the effect they have on the context. If a voxel casts a shadow on the context 50% of the time, this could mean that it causes it on a different building each time, meaning that although the voxel looks “bad”, the net result on the context is negligible. This calculation should also be taken into the growth model (link to growth model). This does mean this script will become even more heavy as it now has to calculate the shadow and skylight blocking each time it grows as well. To solve this, the growth model could be normally ran at first, and then after it has finished going through an evaluation loop to check the shadow and skylight blocking on the context. 


## noise

## greenery
<img src="/img/midterm/greenery.png" style="width:280px;">

## Shafts
<img src="/img/midterm/shafts.png" style="width:280px;">

## Corridors

## entrances
<img src="/img/midterm/entrances.png" style="width:280px;">


## Shortest path
Not sure if needed, but perhaps explain the approach? (for example taking an entire line instead of only one voxel as an entrance)

## KPI's to agent criteria 
explain what our criteria for the agents are and how they relate to the KPI's
The following criteria have been implemented in the agent based simulation:

* Matrix based relations between spaces (Collectivity)
* Sun access (Home quality) + (Sustainability)
* Entrance distance for public, housing, gym, parking and communal spaces (Diversity in audience) 
* The heights of the spaces (Home quality)
* The desired area requirements per space (Program of requirements)
* Skyview (Home quality)
* Greenery (Sustainability)
* Noise (Home quality) + (Collectivity) + (Sustainability)


## Configuring spaces to workable values
In order to implement the design criteria as mentioned before, those had to been converted to workable values. The values have been written for each space versus the criteria varying from 0 to 1. 0 indicates no connection, 1 indicates a strong connection. This has been applied for the following criteria: matrix based relations between spaces, sun access, entrance distance for public, housing, gym, parking and communal spaces, skyview, greenery and noise. 

For the space heights and space areas a different approach had to be made since those are hardcoded criteria coming from the Program of Requirements. Hence, those explained in the next paragraph. 

### Space heights to stencils Tim __> explain different stencil
explain why we are using different stencils 

### Space areas to voxel amounts
Based on the program of requirements, the required space sizes have been coverted to amount of necessary voxels to meet the area requirement and therefore fulfil it. This has been implemented in the script to maintain the desirded area per spaces, and has been used to limit the growth of the agents. 

Explain how script works.

### Definitive program table result (input for generative relations simulation)
The following table has been made based on the agent criterias, this has been used in the definitive script for generating the agent based design. 

<iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vRBfjaAFNv4mAaDcMxI9AJf91QjGnhEDCYvvPLZC6GWHoceZO_pG81HI14bg5hD9g/pubhtml?gid=1256579589&amp;single=true&amp;widget=true&amp;headers=false"style="width:150%; height:600px;"></iframe>

### The simulation

<iframe src="https://thumbs.gfycat.com/LittleAdmiredBufeo-size_restricted.gif" style="width:150%; height:400px;" frameborder="0"></iframe>