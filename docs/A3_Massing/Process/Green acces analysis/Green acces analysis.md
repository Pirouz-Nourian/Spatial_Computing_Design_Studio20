# Green acces analysis

## Explanation

Because the three public green spaces in the nearby neighborhood of the building play such a central role in our project, we also wanted to compute the distance to this spaces, because we think it's important that this space should be as reachable as possible for the people who are using the building.

## Distance lattice

![title](../../../img/Distance_public_green.png)

## Pseudo code

``` python

Input: Final voxelized envelope (high res and low res)

1. Define stencil (Von Neumann neighborhood)

2. Import envelope lattice

3. Distance field construction 
Extract the connectivity graph 
Compute the distances by the Floyd Warshll algorithm and store the distances in a distance matrix
Select three voxel in the envelope that represent a public green space
Construct the distance the three public green spaces

4. Interpolate the distance lattice over low resolution envelope and high resolution envelope

Output: Distance lattice

```

[Atrium allocation and green path finding full notebook](/spatial_computing_project_template/index/scripts/atrium_allocation_and_green_path_finding/)
