# Shaft and Corridor placement
The shafts and corridors have been generated in a way that each floor has a network of corridors without any dead ends. 

<center><img src="https://cdn.discordapp.com/attachments/784009094474366977/803363005420011520/depth_analysis_graphTekengebied_1.png">

<img src="https://media.discordapp.net/attachments/784009094474366977/803363007136137216/depth_analysis_legendTekengebied_1.png" style="width:400px;">

*depth analysis*
</center>

As is shown in the depth analysis (a translation of the topological map, with the focus on connections to the street) it can be seen that all public functions have their own entrance, all communal functions are connected to the community centre and all housing functions are connected to the housing entrance. For this corridor generation these two connection-groups are the connections that are being used. Other connections are considered secondary, and are therefore not taken into account. 
The division in functions (housing entrance and communal entrance) corresponds with the initial location of the functions, where all communal functions are located on the first two floors and the housing functions, along with the co-working spaces and the start-up offices, grow above this. This results in two types of corridor networks; one connecting all communal functions and all shafts to upper floors, and one connecting all functions that grow on the upper floors, along with the shafts.
For the growth of the communal corridors, the initial seeds of these functions are being placed. Because the location of the shafts is more related to the location of the entrances than it is to the seeds of the voxels, the shafts are located based on these entrance locations.

<center><img src="https://raw.githubusercontent.com/EdaAkaltun/spatial_computing_project_template/master/docs/img/finalscreenshots/size_restricted.gif">

*shaft locations based on entrances* 
</center>


Instead of using the shafts as connections towards each seed, a list of each function seed is used. The shaft locations are then added to this list, and after generating a distance matrix the shortest path between each function in this list is then calculated. These shortest paths are calculated in a two-dimensional plane, and are then copied to all corresponding levels; voxels 1, 2 and 3 for the groundfloor level and voxels 4 and 5 for the second floor. 

<center><img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/5.1_corridors_groundfloor.png?raw=true">

*corridors communal functions* 
</center>

The same steps are being taken for the higher level functions, but in addition the height of the corridors is limited by the length of the shaft that it is connected to. This because the length of the shaft determines whether or not the floor is accessible. 

<center><img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/6.4_shafts_and_corridors.png?raw=true">

*corridors upper floors*
</center>

### Corridor evaluation
Because the corridors are all connected, they limit the abm growth by enclosing voxel seeds and therefore we turned them off in this simulation. The location of the shafts led to the corridors growing along the border of the available voxels, occupying valuable space in the building.  The shafts itself are also along the border, but this is a design question that does not have a straightforward answer, as shaft locations differ in each building design. For this situation, a placement along the border had been chosen to minimize walking distance within the building. 

### Improvements
The concept of these corridors is useful, but should be generated after the growth of the functions. Then it can find paths along the borders of each function, making sure no function gets limited in their growth. For some functions it might not be a problem if they were to be split up by these corridors and this could be determined in the agent preferences (housing functions, offices). This method would also make sure the most valuable voxels would not be occupied by the corridors, because the voxels that would now be occupied are the voxels that are placed later in the growth model and are therefore less ideal. Right now the corridor grows towards the most valuable voxel, occupying both this voxel and (at least) two of its direct neighbours.
For the shafts, a stencil should be used to occupy all voxels that would be occupied when placing a shaft. 

