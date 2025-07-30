BP_CUDA
=======

This is a GPU implementation of a simple backprojection algorithm for 2D data sets. It takes projection data as input, and returns the backprojection of this data.

Supported geometries: parallel, parallel_vec, fanflat, fanflat_vec.

Configuration options
---------------------
============================= 	======== 	==
name 				type 		description
=============================	======== 	==
cfg.ProjectionDataId 		required 	`Projection data object ID <../concepts.html#data>`_
cfg.ReconstructionDataId 	required 	`ID of data object <../concepts.html#data>`_ to store the result. The content of this data is overwritten.
cfg.option.GPUindex 		optional 	Specifies which GPU to use. Default = 0.
cfg.option.PixelSuperSampling 	optional 	Specifies the amount of pixel supersampling, i.e., how many (one dimension) subpixels are generated from a single parent pixel.
============================= 	======== 	==

