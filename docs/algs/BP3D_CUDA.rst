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
cfg.ProjectionDataId 			required	`Projection data object ID <../concepts.html#data>`_
cfg.ReconstructionDataId 		required	`ID of data object <../concepts.html#data>`_ to store the result. The content of this data is overwritten.
cfg.option.GPUindex 			optional	The index (zero-based) of the GPU to use. (default: 0)
cfg.option.VoxelSuperSampling 		optional	For the backward projection, VoxelSuperSampling^3 rays will be used. This should only be used if your voxels in the reconstruction volume are larger than the detector pixels. (default: 1)
================================	========	====
