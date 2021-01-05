# Process

> Here you should include the process and product of your 3rd activity: **Massing**

| Title     | Massing (process): Composition (product)                                                                                                                                                                                                                                                                                                                                                                                                                  |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Objective | Logically place the functional spaces in between bridges within the building envelope.                                                                                                                                                                                                                                                                                                                                                                    |
| Procedure | Compute a Solar Envelope, i.e. an envelope of cuboids/voxels, some of which are removed because they are in the way of the neighbouring buildings receiving some standard/minimum level of direct sunlight. Fit the circulation manifold into the solar envelope. From the standing platforms corresponding to functional spaces, grow them into voxel clouds within your voxelated envelope. Colour the voxel clouds according to their functionalities. |

## MCDA
#### MCDA Seed Allocation
| 1. Initial location             |  2. Attraction |  3. Final location |
:-------------------------:|:-------------------------:|:-------------------------:
![MCDA3](../img/placeholder.png)| ![MCDA3](../img/placeholder.png)|![MCDA3](../img/placeholder.png)
<img src="../img/MCDA1.png" width="250">|<img src="../img/MCDA2.png" width="250">|![MCDA3](../img/MCDA3.png)
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

#### MCDA Growth algorithm
| 1. Starting the growth             |  2. Growing |  3. Finished growth |
:-------------------------:|:-------------------------:|:-------------------------:
![Growth algorithm1](../img/Growth algorithm1.png)|![Growth algorithm2](../img/Growth algorithm2.png)|![Growth algorithm3](../img/Growth algorithm3.png)
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

## Spatial behaviours
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
