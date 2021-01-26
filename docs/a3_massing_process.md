# Process

#### MCDA Seed Allocation
| 1. Initial location             |  2. Attraction |  3. Final location |
:-------------------------:|:-------------------------:|:-------------------------:
![Max_depth_1](../img/MCDA_1.png)|![Max_depth_2](../img/MCDA_2.png)|![Max_depth_3](../img/MCDA_3.png)
<img src="../img/growth_0.png" width="250">|<img src="../img/growth_30.png" width="250">|<img src="../img/growth_100.png" width="250">
The location of the seed agents is calculated by looking at the <strong>static environmental data</strong>: Entrance access, street noise, sky view factor, etc.|The different seed agents are attracted to each other, based on the <strong>connectivity matrix</strong>. They ‘walk’ around, until they have reached an ideal location based on internal attraction and external data.|The seed agents have reached an equilibrium.

<table><thead><tr class="header"><th>Pseudocode</th><th></th></tr></thead><tbody><tr class="odd"><td>Input</td><td>Static env-data, preference and connectivity matrix</td></tr><tr class="even"><td>Output</td><td><p>Seed agent positions</p></td></tr>

<tr class="odd"><td>Code</td><td><p>def select-neighbours:
<br>circumvent the encountered bug</p>

<p>def distance-lattice:
<br>calculate the euclidian distance from the seed agent to
every voxel</p>

<p>for each <strong>agent</strong>:
<br>for each voxel:
<br>check if voxel is available:
<br>calculate ‘<strong>grade</strong>’ (based on env-data and agent preference)
<br><strong>append</strong> best voxel to agent list</p>

<p>while t < threshold:
<br>for each <strong>agent</strong>:
<br>calculate a <strong>closeness lattice</strong> to the seed voxel
<br>select-neighbours:
<br>check which neighs are available:
<br><strong>grade</strong> those neighs on dist and env-data
<br><strong>append</strong> best voxel to agent list
<br><strong>remove previous voxel</strong> of this agent
<br>t += 1</p>

</p></td></tr></tbody></table>

#### Squareness 
If there is the need for a space to be <strong>more rectangular<strong>, instead of free-form, the squareness algorithm can be used.

![Squareness](../img/Squareness.png)

<table><thead><tr class="header"><th>Pseudocode</th><th></th></tr></thead><tbody><tr class="odd"><td>Input</td><td>Voxelized envelope, squareness preferences</td></tr><tr class="even"><td>Output</td><td><p>Impacts the growing algorithm</p></td></tr>

<tr class="odd"><td>Code</td><td><p><strong>For each agent</strong> (during the growth process):
<br>Find the <strong>free neighbours</strong> based on the chosen stencil,
<br>Check if the agent has free neighbouring voxels 
<br>Check if those neighbours are also <strong>neighbours of the previous agent</strong></p>

<p>For those voxels that were neighbours to the previous agent, <strong>increase the voxel value</strong> (the more often a voxel has been a neighbour of an agent, the more the voxel value increases)</p> 

<p>Select the neighbour with the <strong>highest voxel value</strong>
<br>Set the selected neighbour as <strong>unavailable</strong>
<br>The selected neighbour is now the <strong>new agent</strong>

</td></tr></tbody></table>

#### Distance between functions 

<img src="../img/keep distance.png" width="350">

<table><thead><tr class="header"><th>Pseudocode</th><th></th></tr></thead><tbody><tr class="odd"><td>Input</td><td>Location of new agent</td></tr><tr class="even"><td>Output</td><td><p>Keep-distance-lattice</p></td></tr>

<tr class="odd"><td>Code</td><td><p>field = [list of neighbours in a given radius]
<br>For i in field: 
<br>keep-distance-lattice[ loc + i ] = agent-ID</p>

<p>### Later on, when determining neighbours</p> 

<p>if keep-distance[neighbour-location] == agent-ID or -1:
<br>Sneighbours . append(neighbours-location)
</td></tr></tbody></table>

#### Maximum building depth

| 1.          |  2.  |  3.  |
:-------------------------:|:-------------------------:|:-------------------------:
![Max_depth_1](../img/Max_depth_1.png)|![Max_depth_2](../img/Max_depth_2.png)|![Max_depth_3](../img/Max_depth_3.png)
<strong>Three directions</strong> are filled.| All <strong>four directions</strong> are filled.|If there is one direction with only three voxels remaining, the fourth voxel is made <strong> unavailable</strong>.


<table><thead><tr class="header"><th>Pseudocode</th><th></th></tr></thead><tbody><tr class="odd"><td>Input</td><td>Agent locations</td></tr><tr class="even"><td>Output</td><td><p>Updates to avail-lattice</p></td></tr>

<tr class="odd"><td>Code</td><td><p>For <strong>each voxel-location</strong> of agent:
<br>check if all voxels in given distance are <strong>occupied in x and y axis</strong>:
<br>check how many axes <strong>don’t</strong> have a <strong>n+1th voxel</strong>:
<br>if amount is 1: 
<br>make remaining n+1th voxel <strong>unavailable</strong></p>
</td></tr></tbody></table>

#### Roof light 

<img src="../img/roof_light.png" width="500">

<table><thead><tr class="header"><th>Pseudocode</th><th></th></tr></thead><tbody><tr class="odd"><td>Input</td><td>New agent locations</td></tr><tr class="even"><td>Output</td><td><p>Updates to avail-lattice</p></td></tr>

<tr class="odd"><td>Code</td><td><p>roof-light = [ list of functions that do not want voxels above them ]</p>
<p><br>if agent-id in roof-light: 
<br>avail-lattice[ neigh-3d-loc[0], neigh-3d-loc[1]] , 2: ] = -1</p>
</td></tr></tbody></table>

#### MCDA Growth algorithm
| 1. Starting the growth             |  2. Growing |  3. Finished growth |
:-------------------------:|:-------------------------:|:-------------------------:
![MCDA_growth_1](../img/MCDA_growth_1.png)|![MCDA_growth_2](../img/MCDA_growth_2.png)|![MCDA_growth_3](../img/MCDA_growth_3.png)
![growth_110](../img/growth_110.png)|![growth_390](../img/growth_390.png)|![growth_1700](../img/growth_1700.png)
All the agent seeds are evaluated and their best neighbour is chosen based on the static <strong>env-data</strong> and <strong>closeness</strong> to other agents. This is done with the connectivity and preference matrix| For each agent, the algorithm evaluates <strong>every voxel</strong> and calculates all the possible neighbors. The best one is chosen.|The max. number of voxels per agent has been reached, the division of the spaces has ended.

<table><thead><tr class="header"><th>Pseudocode</th><th></th></tr></thead><tbody><tr class="odd"><td>Input</td><td>Static env-data, preference and connectivity matrix</td></tr><tr class="even"><td>Output</td><td><p>Occupation lattice</p></td></tr>

<tr class="odd"><td>Code</td><td><p>while t < threshold:
<br>for each <strong>agent</strong>:
<br>check if max. amount of voxels has been reached
<br>for each <strong>agent location</strong>:
<br>find neighs:
<br>check which neighs are available:
<br><strong>grade</strong> those neighs on dist and env-data
<br><strong>append</strong> best voxel to agent list
<br>t += 1

</td></tr></tbody></table>

#### Shafts and corridors growth

| 1. Selecting voxels to evaluate |  2. Finding mean voxels |  3. Mean voxels again | 4. Corridor growth |
:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:
![corridors_1_1](../img/corridors_1_1.png)|![corridors_2_1](../img/corridors_2_1.png)|![corridors_3_1](../img/corridors_3_1.png)|![corridors_4_1](../img/corridors_4_1.png)
![corridors_1](../img/corridors_1.png)|![corridors_2](../img/corridors_2.png)|![corridors_3](../img/corridors_3.png)|![corridors_4](../img/corridors_4.png)
Not all the voxels need a shaft to be placed. The garden for instance would be strange to take into account.| For every function in de occupation lattice, a certain amount of voxels are set, based on the size of each function. Each function has at least 1 mean voxel so that later on corridors can grow and acces all functions.|From the previous mean voxels, new mean voxels are calculated, that will become the shafts inside the new lattice. | Each shaft is connected on the ground floor to the other shafts. Also second corridors grow from each mean voxel to their closest shaft.


<table><thead><tr class="header"><th>Pseudocode</th><th></th></tr></thead><tbody><tr class="odd"><td>Input</td><td>Occupation lattice</td></tr><tr class="even"><td>Output</td><td><p>Shafts and corridors lattice
</p></td></tr>

<tr class="odd"><td>Code</td><td><p>Make a boolean lattice for all important voxels from the occupation
lattice</p>

<p><br><strong>For</strong> each agent:
<br>calculate a number of mean voxels based on the agents occupation</p>

<p><br><strong>For</strong> each mean voxel:
<br>calculate 6 new mean voxels for shaft placement</p>

<p><br><strong>For</strong> each mean voxel:
<br>calculate the closest distance to a shaft
<br>set this path as a corridor</p>

<p><br><strong>For</strong> each shaft:
<br>calculate the 2 closest distances to another shaft on the ground floor
<br>set these paths as corridors</p>

<br>export the shafts and corridors lattice </p>
</td></tr></tbody></table>

#### Floorplan
<img src="../img/Floorplan.PNG" width="500"> 
<img src="../img/Legenda_floorplan.PNG" width="500">
<img src="../img/axo.jpg" width="300">