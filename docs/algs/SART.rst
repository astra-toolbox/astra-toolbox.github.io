SART
====

This is a CPU implementation of the Simultaneous Algebraic Reconstruction Technique (SART) for 2D data sets. It takes projection data and an initial reconstruction as input, and returns the reconstruction after a specified number of SART iterations. Each iteration of SART consists of an FP and BP of one single projection direction. The order of the projections can be specified.

Supported geometries: parallel, fanflat, fanflat_vec, matrix.

Configuration options
---------------------

=============================== ========	=======================================================================================================================================================================================================
name 				type 		description
=============================== ========	=======================================================================================================================================================================================================
cfg.ProjectorId 		required 	The astra_mex_projector ID of the projector.
cfg.ProjectionDataId 		required 	The astra_mex_data2d ID of the projection data
cfg.ReconstructionDataId 	required 	The astra_mex_data2d ID of the reconstruction data. The content of this when starting SART is used as the initial reconstruction.
cfg.option.SinogramMaskId 	optional 	If specified, the astra_mex_data2d ID of a projection-data-sized volume to be used as a mask.
cfg.option.ReconstructionMaskId optional 	If specified, the astra_mex_data2d ID of a volume-data-sized volume to be used as a mask.
cfg.option.MinConstraint 	optional 	If specified, all values below MinConstraint will be set to MinConstraint. This can, for example, be used to enforce non-negative reconstructions.
cfg.option.MaxConstraint 	optional 	If specified, all values above MaxConstraint will be set to MaxConstraint.
cfg.option.ProjectionOrder 	optional 	This specifies the order in which the projections are used. Possible values are: 'random' (default), 'sequential', and 'custom'. If 'custom' is specified, the option.ProjectionOrderList is required.
cfg.option.ProjectionOrderList 	optional 	Required if option.ProjectionOrder = 'custom', ignored otherwise. A matlab vector containing the custom order in which the projections are used.
=============================== ========	=======================================================================================================================================================================================================

Example
-------

.. code-block:: matlab

	%% create phantom
	V_exact = phantom(256);

	%% create geometries and projector
	proj_geom = astra_create_proj_geom('parallel', 1.0, 256, linspace2(0,pi,180));
	vol_geom = astra_create_vol_geom(256,256);
	proj_id = astra_create_projector('linear', proj_geom, vol_geom);

	%% create forward projection
	[sinogram_id, sinogram] = astra_create_sino(V_exact, proj_id);

	%% reconstruct
	recon_id = astra_mex_data2d('create', '-vol', vol_geom, 0);
	cfg = astra_struct('SART');
	cfg.ProjectorId = proj_id;
	cfg.ProjectionDataId = sinogram_id;
	cfg.ReconstructionDataId = recon_id;
	cfg.option.ProjectionOrder = 'custom';
	cfg.option.ProjectionOrderList = [0:5:175 1:5:176 2:5:177 3:5:178 4:5:179];
	sart_id = astra_mex_algorithm('create', cfg);
	astra_mex_algorithm('iterate', sart_id, 10*180);
	V = astra_mex_data2d('get', recon_id);
	imshow(V, []);

	%% garbage disposal
	astra_mex_data2d('delete', sinogram_id, recon_id);
	astra_mex_projector('delete', proj_id);
	astra_mex_algorithm('delete', sart_id);

Further examples regarding the different projection orders can be found in example_sart_order.m .
