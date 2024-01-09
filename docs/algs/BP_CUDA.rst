BP_CUDA
=======

This is a GPU implementation of a simple backprojection algorithm for 2D data sets. It takes projection data as input, and returns the backprojection of this data.

Supported geometries: parallel, fanflat, fanflat_vec.

Configuration options
---------------------
============================= 	======== 	==
name 				type 		description
=============================	======== 	==
cfg.ProjectionDataId 		required 	The astra_mex_data2d ID of the projection data
cfg.ReconstructionDataId 	required 	The astra_mex_data2d ID for the backprojection data.
cfg.option.GPUindex 		optional 	Specifies which GPU to use. Default = 0.
cfg.option.PixelSuperSampling 	optional 	Specifies the amount of pixel supersampling, i.e., how many (one dimension) subpixels are generated from a single parent pixel.
============================= 	======== 	==

