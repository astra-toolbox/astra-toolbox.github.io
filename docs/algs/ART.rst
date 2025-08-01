ART
===

This is a CPU implementation of the Iterative Reconstruction Technique (ART) for 2D data sets. It takes projection data and an initial reconstruction as input, and returns the reconstruction after a specified number of ART iterations. Each iteration of ART consists of an FP and BP of one single projection ray. The order of the rays can be specified.

Supported geometries: parallel, parallel_vec, fanflat, fanflat_vec, matrix.

Configuration options
---------------------

.. list-table::
  :header-rows: 1

  * - Name
    - Description

  * - ProjectorId
    - `Projector object ID <../proj2d.html>`_.

  * - ProjectionDataId
    - `Projection data object ID <../concepts.html#data>`_.

  * - ReconstructionDataId
    - `ID of data object <../concepts.html#data>`_ to store the result. The
      content of this when starting ART is used as the initial reconstruction.

  * - *option.RayOrder*
    - This specifies the order in which the projections are used. Possible
      values are: 'sequential' (default) and 'custom'. If 'custom' is specified,
      the option.RayOrderList is required.

  * - *option.RayOrderList*
    - Required if option.RayOrder = 'custom', ignored otherwise. An 1D array
      containing the custom order in which the projections are used.

  * - *option.MinConstraint*
    - If specified, all values below MinConstraint will be set to MinConstraint.
      This can, for example, be used to enforce non-negative reconstructions.

  * - *option.MaxConstraint*
    - If specified, all values above MaxConstraint will be set to MaxConstraint.

  * - *option.SinogramMaskId*
    - If specified, `data object ID <../concepts.html#data>`_ of a
      projection-data-sized volume to be used as a `mask <../misc.html#masks>`_.

  * - *option.ReconstructionMaskId*
    - If specified, `data object ID <../concepts.html#data>`_ of a
      volume-data-sized volume to be used as a `mask <../misc.html#masks>`_.


Example
-------

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      import astra
      import matplotlib.pyplot as plt
      import numpy as np

      # Create geometries and projector
      N = 256
      N_ANGLES = 180
      det_spacing = 1.0
      angles = np.linspace(0, np.pi, N_ANGLES)
      proj_geom = astra.create_proj_geom('parallel', det_spacing, N, angles)
      vol_geom = astra.create_vol_geom(N, N)
      projector_id = astra.create_projector('linear', proj_geom, vol_geom)

      # Generate phantom image
      phantom_id, phantom = astra.data2d.shepp_logan(vol_geom)

      # Create forward projection
      sinogram_id, sinogram = astra.create_sino(phantom_id, projector_id)

      # Reconstruct
      recon_id = astra.data2d.create('-vol', vol_geom)
      cfg = astra.astra_dict('ART')
      cfg['ProjectorId'] = projector_id
      cfg['ProjectionDataId'] = sinogram_id
      cfg['ReconstructionDataId'] = recon_id
      cfg['option'] = {'RayOrder':  'custom'}
      # Set ray order to a random permutation (equivalent to cfg['option']['RayOrder'] = 'random')
      full_ray_list = np.stack(np.meshgrid(range(N_ANGLES), range(N))).reshape(2, -1).T
      cfg['option']['ray_order_list'] = np.random.permutation(full_ray_list).flatten()
      algorithm_id = astra.algorithm.create(cfg)

      astra.algorithm.run(algorithm_id, 3 * sinogram.size)

      recon = astra.data2d.get(recon_id)
      plt.imshow(recon, cmap='gray')

      # Clean up
      astra.data2d.delete([sinogram_id, recon_id, phantom_id])
      astra.projector.delete(projector_id)
      astra.algorithm.delete(algorithm_id)


  .. group-tab:: MATLAB
    .. code-block:: matlab

      %% Create phantom
      N = 256;
      phantom = phantom(N);

      %% Create geometries and projector
      det_spacing = 1.0;
      N_ANGLES = 180;
      angles = linspace(0, pi, N_ANGLES);
      proj_geom = astra_create_proj_geom('parallel', det_spacing, N, angles);
      vol_geom = astra_create_vol_geom(N, N);
      projector_id = astra_create_projector('linear', proj_geom, vol_geom);

      %% Create forward projection
      [sinogram_id, sinogram] = astra_create_sino(phantom, projector_id);

      %% Reconstruct
      recon_id = astra_mex_data2d('create', '-vol', vol_geom);
      cfg = astra_struct('ART');
      cfg.ProjectorId = projector_id;
      cfg.ProjectionDataId = sinogram_id;
      cfg.ReconstructionDataId = recon_id;
      cfg.option.RayOrder = 'custom';
      % Set ray order to a random permutation (equivalent to cfg.option.RayOrder = 'random')
      [p, q] = meshgrid(0:(N_ANGLES-1), 0:(N-1));
      ray_order_list = [p(:) q(:)];
      ray_order_list = ray_order_list(randperm(numel(p)), :);
      cfg.option.ray_order_list = ray_order_list;
      algorithm_id = astra_mex_algorithm('create', cfg);

      astra_mex_algorithm('iterate', algorithm_id, 3 * numel(p));

      recon = astra_mex_data2d('get', recon_id);
      imshow(recon, []);

      %% Clean up
      astra_mex_data2d('delete', sinogram_id, recon_id);
      astra_mex_projector('delete', projector_id);
      astra_mex_algorithm('delete', algorithm_id);
