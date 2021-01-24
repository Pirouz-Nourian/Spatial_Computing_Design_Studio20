# Sun analysis
### Explanation

With the sun access analysis we wanted to gain insight into the amount of sunlight  on different places in the building. We want to use this information for placing the different agents in an optimal place.

We have started this analysis by importing the final envelope and are again computing intersections between rays and the context. Now we are actually shooting rays upwards. When an intersection between the ray and the context is found, the voxel does not fully see the sun and thus, has a lower percentage of sun access.

### Envelope

![Title](../../../img/sun.jpg)

### Pseudo code

``` python
Input: voxelized envelope csv
(low and high res), context mesh

1. Import Meshes

2. Import Lattice

3. Import Sun Vectors
import sunpath (ladybug) coordinates Rotterdam
reduce the number of sun vectors (days and hours)

4. Compute intersection 
sun direction = - sun_vectors
find the centroids of the voxels
shoot rays from all of the centroids in all of the sun direction
find intersections of the rays with the context mesh 

5. Make Sun Access lattice
make a list out of the rays that had an intersection 
initiate the list of ratio 
	iterate over all of the voxels 
	count the intersection
		iterate over all of the sun rays 
	compute the percentage of rays that did not intersect 
    (could see the sun)
	add to the list = vox_sun_access

6. Interpolate vox_sun_access (lattice) and voxelized envelope csv (high res) 

Output: sun access csv (low and high res)

```

[Sun analysis full python code](/notebooks/sun/)


