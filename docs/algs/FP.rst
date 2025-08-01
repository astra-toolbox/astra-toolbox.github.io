FP
==

This is a CPU implementation of a simple forward projection algorithm for 2D data sets. It takes a projector and a volume as input, and returns the projection data.

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
    - `Data object ID <../concepts.html#data>`_ to store the result. The
      computed forward projection is added to this data.

  * - VolumeDataId
    - `Volume data object ID <../concepts.html#data>`_.

  * - *option.SinogramMaskId*
    - If specified, `data object ID <../concepts.html#data>`_ of a
      projection-data-sized volume to be used as a `mask <../misc.html#masks>`_.

  * - *option.VolumeMaskId*
    - If specified, the `data object ID <../concepts.html#data>`_ of a
      volume-data-sized volume to be used as a mask.


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

      # Calculate forward projection
      projection_id = astra.data2d.create('-sino', proj_geom)
      cfg = astra.astra_dict('FP')
      cfg['ProjectorId'] = projector_id
      cfg['ProjectionDataId'] = projection_id
      cfg['VolumeDataId'] = phantom_id
      algorithm_id = astra.algorithm.create(cfg)

      astra.algorithm.run(algorithm_id)

      projection = astra.data2d.get(projection_id)
      plt.imshow(projection, cmap='gray')

      # Clean up
      astra.data2d.delete([projection_id, phantom_id])
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

      %% Calculate forward projection
      phantom_id = astra_mex_data2d('create', '-vol', vol_geom, phantom);
      projection_id = astra_mex_data2d('create', '-sino', proj_geom);
      cfg = astra_struct('FP');
      cfg.ProjectorId = projector_id;
      cfg.ProjectionDataId = projection_id;
      cfg.VolumeDataId = phantom_id;
      algorithm_id = astra_mex_algorithm('create', cfg);

      astra_mex_algorithm('run', algorithm_id);

      projection = astra_mex_data2d('get', projection_id);
      imshow(projection, []);

      %% Clean up
      astra_mex_data2d('delete', phantom_id, projection_id);
      astra_mex_projector('delete', projector_id);
      astra_mex_algorithm('delete', algorithm_id);
