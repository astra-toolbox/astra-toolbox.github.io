BP
==

This is a CPU implementation of a simple backprojection algorithm for 2D data sets. It takes a projector and projection data as input, and returns the backprojection of this data.

Supported geometries: parallel, parallel_vec, fanflat, fanflat_vec, matrix.

Configuration options
---------------------
=============================== ======== 	===============================================================================================
name 				type 		description
=============================== ======== 	===============================================================================================
cfg.ProjectorId 		required 	`Projector object ID <../proj2d.html>`_
cfg.ProjectionDataId 		required 	`Projection data object ID <../concepts.html#data>`_
cfg.ReconstructionDataId 	required 	`ID of data object <../concepts.html#data>`_ to store the result. The content of this data is overwritten.
cfg.option.SinogramMaskId 	optional 	If specified, `data object ID <../concepts.html#data>`_ of a projection-data-sized volume to be used as a mask.
cfg.option.ReconstructionMaskId optional 	If specified, `data object ID <../concepts.html#data>`_ of a volume-data-sized volume to be used as a mask.
=============================== ======== 	===============================================================================================
