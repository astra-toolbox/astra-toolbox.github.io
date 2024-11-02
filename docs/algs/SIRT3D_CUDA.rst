SIRT3D_CUDA
===========

This is a GPU implementation of the SIRT algorithm for 3D data sets.
It takes projection data and an initial reconstruction as input, and
returns the reconstruction after a specified number of SIRT iterations.

Supported geometries: all 3D geometries.

Configuration options
---------------------

================================	========	====
name 					type 		description
================================	========	====
cfg.ProjectionDataId 			required	The astra_mex_data3d ID of the projection data
cfg.ReconstructionDataId 		required	The astra_mex_data3d ID of the reconstruction data. The content of this when starting SIRT3D_CUDA is used as the initial reconstruction.
cfg.option.SinogramMaskId 		optional	If specified, the astra_mex_data3d ID of a projection-data-sized volume to be used as a mask. It should only have values 0.0 and 1.0. See the section on [Masks] for details.
cfg.option.ReconstructionMaskId 	optional	If specified, the astra_mex_data3d ID of a volume-data-sized volume to be used as a mask. It should only have values 0.0 and 1.0. See the section on [Masks] for details.
cfg.option.MinConstraint 		optional	If specified, all values below MinConstraint will be set to MinConstraint. This can be used to enforce non-negative reconstructions, for example.
cfg.option.MaxConstraint 		optional	If specified, all values above MaxConstraint will be set to MaxConstraint.
cfg.option.GPUindex 			optional	The index (zero-based) of the GPU to use. (default: 0)
cfg.option.DetectorSuperSampling 	optional	For the forward projection, DetectorSuperSampling^2 rays will be used. This should only be used if your detector pixels are larger than the voxels in the reconstruction volume. (default: 1)
cfg.option.VoxelSuperSampling 		optional	For the backward projection, VoxelSuperSampling^3 rays will be used. This should only be used if your voxels in the reconstruction volume are larger than the detector pixels. (default: 1)
================================	========	====

Extra features
--------------

SIRT3D_CUDA supports astra.algorithm.get_res_norm() / astra_mex_algorithm('get_res_norm') to get
the 2-norm of the difference between the projection data and the projection of the reconstruction.
(The square root of the sum of squares of differences.)

