BP
==

This is a CPU implementation of a simple backprojection algorithm for 2D data sets. It takes a projector and projection data as input, and returns the backprojection of this data.

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
      content of this data is overwritten.

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

      # Calculate backprojection
      backprojection_id = astra.data2d.create('-vol', vol_geom)
      cfg = astra.astra_dict('BP')
      cfg['ProjectorId'] = projector_id
      cfg['ProjectionDataId'] = sinogram_id
      cfg['ReconstructionDataId'] = backprojection_id
      algorithm_id = astra.algorithm.create(cfg)

      astra.algorithm.run(algorithm_id)

      backprojection = astra.data2d.get(backprojection_id)
      plt.imshow(backprojection, cmap='gray')

      # Clean up
      astra.data2d.delete([sinogram_id, backprojection_id, phantom_id])
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

      %% Calculate backprojection
      backprojection_id = astra_mex_data2d('create', '-vol', vol_geom);
      cfg = astra_struct('BP');
      cfg.ProjectorId = projector_id;
      cfg.ProjectionDataId = sinogram_id;
      cfg.ReconstructionDataId = backprojection_id;
      algorithm_id = astra_mex_algorithm('create', cfg);

      astra_mex_algorithm('run', algorithm_id);

      backprojection = astra_mex_data2d('get', backprojection_id);
      imshow(backprojection, []);

      %% Clean up
      astra_mex_data2d('delete', sinogram_id, backprojection_id);
      astra_mex_projector('delete', projector_id);
      astra_mex_algorithm('delete', algorithm_id);
