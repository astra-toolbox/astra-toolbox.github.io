FDK_CUDA
========

This is a GPU implementation of the FDK algorithm for 3D circular cone beam data sets. It takes
projection data as input, and returns the reconstruction.

Supported geometries: cone, cone_vec.

**Note:** cone_vec geometries that significantly deviate from
a circular scanning geometry will likely lead to artifacts in the reconstruction.

Configuration options
---------------------
================================	========	====
name 					type 		description
================================	========	====
cfg.ProjectionDataId 			required 	The astra_mex_data3d ID of the projection data
cfg.ReconstructionDataId 		required	The astra_mex_data3d ID of the reconstruction data.
cfg.option.ShortScan 			optional	If enabled, do Parker weighting to support non-360-degree data. This needs an angle range of at least 180 degrees plus twice the cone angle. (default: false)
cfg.option.GPUindex 			optional	The index (zero-based) of the GPU to use. (default: 0)
cfg.option.VoxelSuperSampling 		optional	For the backward projection, VoxelSuperSampling^3 rays will be used. This should only be used if your voxels in the reconstruction volume are larger than the detector pixels. (default: 1)
================================	========	====
