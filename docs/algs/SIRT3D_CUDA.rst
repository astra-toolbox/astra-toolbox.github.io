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
cfg.ProjectionDataId 			required	`Projection data object ID <../concepts.html#data>`_
cfg.ReconstructionDataId 		required	`ID of data object <../concepts.html#data>`_ to store the result The content of this data is used as the initial reconstruction.
cfg.option.SinogramMaskId 		optional	If specified, `data object ID <../concepts.html#data>`_ of a projection-data-sized volume to be used as a mask. It should only have values 0.0 and 1.0. See the section on `masks <../misc.html#masks>`_ for details.
cfg.option.ReconstructionMaskId 	optional	If specified, `data object ID <../concepts.html#data>`_ of a volume-data-sized volume to be used as a mask. It should only have values 0.0 and 1.0. See the section on `masks <../misc.html#masks>`_ for details.
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

