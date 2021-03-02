# Voxelizing the lattice

As the final voxelsize is determined, the location can be voxelized. The given 'compulsory envelope' is being filled with voxels sized 1.8m * 1.8m * 1.8m, which results in the full lattice. 

<img src="https://cdn.discordapp.com/attachments/784009094474366977/803578469933252658/mesh.png">

*compulsory envelope*

<img src="https://cdn.discordapp.com/attachments/784009094474366977/803578468969086976/ful_lowres_lattice.png">

*voxelized envelope* 



Because of the relatively small voxelsize, next to this a second lattice is being generated, with voxelsizes of 15m *15m *15m. This 'low-resolution' lattice could later be used to make the more heavy calculations, and by interpolation of these values a good estimate of the 'high-resolution' voxels is being generated.  

<img src="https://cdn.discordapp.com/attachments/784009094474366977/803578466447786074/ful_lattice.png">

*low-resolution voxelized envelope*
