BP
==

This is a CPU implementation of a simple backprojection algorithm for 2D data sets. It takes a projector and projection data as input, and returns the backprojection of this data.

Supported geometries: parallel, parallel_vec, fanflat, fanflat_vec, matrix.

Configuration options
---------------------
=============================== ======== 	===============================================================================================
name 				type 		description
=============================== ======== 	===============================================================================================
cfg.ProjectorId 		required 	The astra_mex_projector ID of the projector.
cfg.ProjectionDataId 		required 	The astra_mex_data2d ID of the projection data
cfg.ReconstructionDataId 	required 	The astra_mex_data2d ID of the reconstruction data. The content of this data is overwritten.
cfg.option.SinogramMaskId 	optional 	If specified, the astra_mex_data2d ID of a projection-data-sized volume to be used as a mask.
cfg.option.ReconstructionMaskId optional 	If specified, the astra_mex_data2d ID of a volume-data-sized volume to be used as a mask.
=============================== ======== 	===============================================================================================
