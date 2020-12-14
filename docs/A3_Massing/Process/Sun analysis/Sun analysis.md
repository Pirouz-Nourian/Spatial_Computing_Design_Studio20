# Sun analysis

With the sun analysis we first wanted to gain insight into the sunny and not sunny places of the building. This analysis of the sun acces on each part of the building is usefull when placing different agents in an optimal place. 

With the computed sun rays and the given context mesh we have computed the intersection between them. When an intersection is found, the calculated voxel does not fully see the sun and thus, has a low percentage of sun access. 

We wanted our building to stay on the ground and not remove all of the lower voxels to keep a good connection between the shops and the hofbogen. For this reason, we have removed the voxels that have less than 55 percent of access to the sun. 

![sun access](../img/sun1.jpg)

