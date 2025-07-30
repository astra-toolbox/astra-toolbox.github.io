CGLS3D_CUDA
===========

This is a GPU implementation of the CGLS algorithm for 3D data sets.
It takes projection data and an initial reconstruction as input, and
returns the reconstruction after a specified number of CGLS iterations.

The internal state of the CGLS algorithm is reset every time
``astra.algorithm.run``/``astra_mex_algorithm('iterate')`` is called. This
implies that running CGLS for N iterations and then running it for another N
iterations may yield different results from running it 2N iterations at once.

Supported geometries: all 3D geometries.

Configuration options
---------------------

================================	========	====
name 					type 		description
================================	========	====
cfg.ProjectionDataId 			required	`Projection data object ID <../concepts.html#data>`_
cfg.ReconstructionDataId 		required	`ID of data object <../concepts.html#data>`_ to store the result. The content of this data is used as the initial reconstruction.
cfg.option.ReconstructionMaskId 	optional	If specified, the `data object ID <../concepts.html#data>`_ of a volume-data-sized volume to be used as a mask. It should only have values 0.0 and 1.0. See the section on [Masks] for details.
cfg.option.GPUindex 			optional	The index (zero-based) of the GPU to use. (default: 0)
cfg.option.DetectorSuperSampling 	optional	For the forward projection, DetectorSuperSampling^2 rays will be used. This should only be used if your detector pixels are larger than the voxels in the reconstruction volume. (default: 1)
cfg.option.VoxelSuperSampling 		optional	For the backward projection, VoxelSuperSampling^3 rays will be used. This should only be used if your voxels in the reconstruction volume are larger than the detector pixels. (default: 1)
================================	========	====

Extra features
--------------

CGLS3D_CUDA supports ``astra.algorithm.get_res_norm()`` /
``astra_mex_algorithm('get_res_norm')`` to get the 2-norm of the difference
between the projection data and the projection of the reconstruction. (The
square root of the sum of squares of differences.)
