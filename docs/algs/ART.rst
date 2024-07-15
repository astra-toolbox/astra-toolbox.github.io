ART
===

This is a CPU implementation of the Iterative Reconstruction Technique (ART) for 2D data sets. It takes projection data and an initial reconstruction as input, and returns the reconstruction after a specified number of ART iterations. Each iteration of ART consists of an FP and BP of one single projection ray. The order of the rays can be specified.

Supported geometries: parallel, fanflat, fanflat_vec, matrix.

Configuration options
---------------------
=============================== ========	================================================================================
name 				type 		description
=============================== ========	================================================================================
cfg.ProjectorId 		required 	The astra_mex_projector ID of the projector.
cfg.ProjectionDataId 		required 	The astra_mex_data2d ID of the projection data
cfg.ReconstructionDataId 	required 	The astra_mex_data2d ID of the reconstruction data. The content of this when starting ART is used as the initial reconstruction.
cfg.option.SinogramMaskId 	optional 	If specified, the astra_mex_data2d ID of a projection-data-sized volume to be used as a mask.
cfg.option.ReconstructionMaskId optional 	If specified, the astra_mex_data2d ID of a volume-data-sized volume to be used as a mask.
cfg.option.MinConstraint 	optional 	If specified, all values below MinConstraint will be set to MinConstraint. This can, for example, be used to enforce non-negative reconstructions.
cfg.option.MaxConstraint 	optional 	If specified, all values above MaxConstraint will be set to MaxConstraint.
cfg.option.RayOrder 		optional 	This specifies the order in which the projections are used. Possible values are: 'sequential' (default) and 'custom'. If 'custom' is specified, the option.RayOrderList is required.
cfg.option.RayOrderList 	optional 	Required if option.RayOrder = 'custom', ignored otherwise. A matlab vector containing the custom order in which the projections are used.
=============================== ========	================================================================================

Example
-------

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      import astra
      import matplotlib.pyplot as plt
      import numpy

      # create geometries and projector
      proj_geom = astra.create_proj_geom('parallel', 1.0, 256, numpy.linspace(0, numpy.pi, 180, endpoint=False))
      vol_geom = astra.create_vol_geom(256,256)
      proj_id = astra.create_projector('linear', proj_geom, vol_geom)

      # generate phantom image
      V_exact_id, V_exact = astra.data2d.shepp_logan(vol_geom)

      # create forward projection
      sinogram_id, sinogram = astra.create_sino(V_exact, proj_id)

      # reconstruct
      recon_id = astra.data2d.create('-vol', vol_geom, 0)
      cfg = astra.astra_dict('ART')
      cfg['ProjectorId'] = proj_id
      cfg['ProjectionDataId'] = sinogram_id
      cfg['ReconstructionDataId'] = recon_id
      cfg['option'] = { }
      cfg['option']['RayOrder'] =  'custom'
      fullRayList = numpy.array(numpy.meshgrid(range(180), range(256))).reshape(2,-1).T
      cfg['option']['RayOrderList'] = numpy.random.permutation(fullRayList).reshape(-1)

      art_id = astra.algorithm.create(cfg)
      astra.algorithm.run(art_id, 3*180*256)
      V = astra.data2d.get(recon_id)
      plt.gray()
      plt.imshow(V)
      plt.show()

      # garbage disposal
      astra.data2d.delete([sinogram_id, recon_id, V_exact_id])
      astra.projector.delete(proj_id)
      astra.algorithm.delete(art_id)


  .. group-tab:: Matlab
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

	[p,q] = meshgrid(0:179, 0:255);
	rayOrderList = [p(:) q(:)];
	rayOrderList = rayOrderList(randperm(numel(p)),:);

	recon_id = astra_mex_data2d('create', '-vol', vol_geom, 0);
	cfg = astra_struct('ART');
	cfg.ProjectorId = proj_id;
	cfg.ProjectionDataId = sinogram_id;
	cfg.ReconstructionDataId = recon_id;
	cfg.option.RayOrder = 'custom';
	cfg.option.RayOrderList = rayOrderList;
	art_id = astra_mex_algorithm('create', cfg);
	astra_mex_algorithm('iterate', art_id, 3*numel(p));
	V = astra_mex_data2d('get', recon_id);
	imshow(V, []);

	%% garbage disposal
	astra_mex_data2d('delete', sinogram_id, recon_id);
	astra_mex_projector('delete', proj_id);
	astra_mex_algorithm('delete', art_id);

Further examples regarding the different projection orders can be found in example_art_order.m .
