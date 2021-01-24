# Atrium allocation 

Before we dive into the several analyses we did, we first want to implement our concept into our model because this will substantially change the shape of our building.

![title](../../../img/Flowchart_atrium.png)

 To do this we first allocated a voxel in the center of the building, by first calculating the length of the x-direction, and then calculating y-direction of the voxelized envelope, and dividing both by 2. The center of the building represents the heart and will eventually be the atrium. 

![title](../../../img/Atrium_center.png)

Now we need to connect the hart to the three public green spaces. This we do by the a-star algorithm to find the shortest path to the center of the building. To find it's way through the building the algorithm takes the Moore neighborhood into acount, so it can grow diagonally. 

![title](../../../img/Atrium and green path_onbewerkt.png)