# Agent growth 

### Explanation

![Title](../../../img/ABM_1.png)

![Title](../../../img/ABM_2.png)

### Pseudo code

``` python

1. Create stencil z & setncil xy
2. While the frame count is below the maximumk of 620
NOISE
2.1 - Check silent level & noise repel
DYNAMIC RELATIONS
2.2 - For every agent
2.2.1 - Find location
2.2.2 - Find distance from the voxel to every other
voxel
2.2.3 - Create a distance lattice in agn_acc_list
2.3 Combine distance lattice & agn_acc_list
2.4 - For every agent
2.4.1 - Find current location
2.4.2 - For every location
STENCILS/HORIZONTAL GROWTH
- Check neighs in z plane & neighs in xy plane
- Compare preference to grow in certain plane
and give them a value. XY = 1, Z = variable
- If neigbour is available add it to list of
free neighbours
2.4.3 - As long as there are free neighbours
- Evaluate every neighbour
- Compare agent preference with enviroment
info
SQUARENESS (Credit to Maren)
- Double neighbours get a higher value
VOXEL COUNT
- If the current voxel count is lower than
the maximum
- Add selected neighbour to list of agent
locations & set neighbour as unavailable
- Else stop
```

[Seed allocation and Agent growth full notebook](/spatial_computing_project_template/index/scripts/abm/)

<span style="color: #76AB24;">* Unfortunately due to limited time we have not been able to debug our current agent based model. When done with the old envelope there were no problems but with the new one some problems occur. What happends in the current model is that some parts of the new envelope are not able to be occupied during the growth process. This leaves some voxels that are occupied during the seed allocation isolated and that function then grows into places is doesn’t want to which makes no sense. This does not correspond to our data and our concept. Besides the debugging there are other improvements that could be made.*</span>

<span style="color: #76AB24;">*1.	Add a max depth criteria for each function, creating daylight access for each housing unit for example. Another idea could be to add skylights or courtyards per x amount of voxels for some functions.*</span> 

<span style="color: #76AB24;">*2.	Add floor preference and closeness to façade (outer & inner)*</span>

<span style="color: #76AB24;">*3.	Have agents negotiate with each other.*</span>

