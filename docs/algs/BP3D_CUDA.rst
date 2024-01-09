BP3D_CUDA
=========

This is a GPU implementation of a simple backprojection algorithm for 3D data
sets. It takes projection data as input, and returns the backprojection of this
data.

Configuration options
---------------------

================================	========	====
name 					type 		description
================================	========	====
cfg.ProjectionDataId 			required	The astra_mex_data3d ID of the projection data
cfg.ReconstructionDataId 		required	The astra_mex_data3d ID of the reconstruction data. The content of this when starting SIRT3D_CUDA is used as the initial reconstruction.
cfg.option.GPUindex 			optional	The index (zero-based) of the GPU to use. (default: 0)
cfg.option.VoxelSuperSampling 		optional	For the backward projection, VoxelSuperSampling^3 rays will be used. This should only be used if your voxels in the reconstruction volume are larger than the detector pixels. (default: 1)
================================	========	====
