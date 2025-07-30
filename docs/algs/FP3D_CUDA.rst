FP3D_CUDA
=========

This is a GPU implementation of a simple forward projection algorithm for 3D data sets. It takes a volume as input, and returns the projection data.

Supported geometries: parallel3d, parallel3d_vec, cone, cone_vec.

Configuration options
---------------------
================================	========	==================================
name 					type 		description
================================	========	==================================
cfg.ProjectionDataId 			required 	`Data object ID <../concepts.html#data>`_ to store the result. The computed forward projection is added to this data.
cfg.VolumeDataId 			required 	`Volume data object ID <../concepts.html#data>`_
cfg.option.DetectorSuperSampling 	optional 	Specifies the amount of detector supersampling, i.e., how many rays are cast per detector.
cfg.option.GPUindex 			optional 	Specifies which GPU to use. Default = 0.
================================	========	==================================
