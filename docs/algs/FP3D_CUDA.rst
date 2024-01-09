FP3D_CUDA
=========

This is a GPU implementation of a simple forward projection algorithm for 3D data sets. It takes a volume as input, and returns the projection data.

Supported geometries: parallel3d, parallel3d_vec, cone, cone_vec.

Configuration options
---------------------
================================	========	==================================
name 					type 		description
================================	========	==================================
cfg.ProjectionDataId 			required 	The astra_mex_data2d ID of the projection data. The forward projection is added to this data.
cfg.VolumeDataId 			required 	The astra_mex_data2d ID of the reconstruction data.
cfg.option.DetectorSuperSampling 	optional 	Specifies the amount of detector supersampling, i.e., how many rays are cast per detector.
cfg.option.GPUindex 			optional 	Specifies which GPU to use. Default = 0.
================================	========	==================================
