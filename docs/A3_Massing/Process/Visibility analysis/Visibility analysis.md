# Visibility analysis 

One of our starting points is to create a building that makes different connections. By this we mean connections between the various surrounding neighborhoods, between the greenery and connections inside of the building. The building should therefore not serve as a wall, but more as a passage. That is why we think it is important that for people passing by, there is a good view of the sky and the surroundings. 
For this, we have calculated the sky view factor and, just like in the previous analysis, removed the unnecessary voxels.

Other than in the sun and shadow analysis we needed to compute the sky vectors. 
``` python
# Compute the sky vectors
sphere_mesh = tm.creation.icosphere(subdivisions=3, radius=1.0, color=None)
sphere_vectors = np.copy(sphere_mesh.vertices)

sky_vectors = []
for v in sphere_vectors:
    if v[2] > 0.0:
        sky_vectors.append(v)

sky_vectors = np.array(sky_vectors)
print(sky_vectors.shape)

```
Then we have computed the intersections between the sky rays and the context. The context exists only of the street around the building. This is because we only need to check if people walking by can not see the sky because of the building we are making. The other context can be left out.

![Title](../img/street.png)

We have done the calculation on the envelope on the envelope that remained after the sun and shadow analysis. 

***Visibility analysis***

![title](../img/svf1.jpg)


***Final envelope***

![title](../img/svf2.jpg)

