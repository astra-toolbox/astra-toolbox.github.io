FP3D_CUDA
=========

This is a GPU implementation of a simple forward projection algorithm for 3D data sets. It takes a volume as input, and returns the projection data.

Supported geometries: parallel3d, parallel3d_vec, cone, cone_vec.

Configuration options
---------------------

.. list-table::
  :header-rows: 1

  * - Name
    - Description

  * - ProjectionDataId
    - `Data object ID <../concepts.html#data>`_ to store the result. The
      computed forward projection is added to this data.

  * - VolumeDataId
    - `Volume data object ID <../concepts.html#data>`_.

  * - *option.DetectorSuperSampling*
    - Each detector pixel will be subdivided by this factor along each
      dimension. This should only be used if detector pixels are larger than the
      voxels in the volume (default: 1).

  * - *option.GPUindex*
    - The index of the GPU to use (default: 0).


Example
-------

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      import astra
      import matplotlib.pyplot as plt
      import numpy as np

      # Create geometries
      N = 256
      N_ANGLES = 180
      det_spacing = 1.0
      angles = np.linspace(0, np.pi, N_ANGLES)
      proj_geom = astra.create_proj_geom('parallel3d', det_spacing, det_spacing, N, N, angles)
      vol_geom = astra.create_vol_geom(N, N, N)

      # Generate phantom image
      phantom_id, phantom = astra.data3d.shepp_logan(vol_geom)

      # Calculate forward projection
      projection_id = astra.data3d.create('-sino', proj_geom)
      cfg = astra.astra_dict('FP3D_CUDA')
      cfg['ProjectionDataId'] = projection_id
      cfg['VolumeDataId'] = phantom_id
      algorithm_id = astra.algorithm.create(cfg)

      astra.algorithm.run(algorithm_id)

      projection = astra.data3d.get(projection_id)
      plt.imshow(projection[N//2], cmap='gray')

      # Clean up
      astra.data3d.delete([projection_id, phantom_id])
      astra.algorithm.delete(algorithm_id)


  .. group-tab:: MATLAB
    .. code-block:: matlab

      %% Create phantom
      N = 256;
      phantom_ = repmat(phantom(N), [1, 1, N]);

      %% Create geometries
      det_spacing = 1.0;
      N_ANGLES = 180;
      angles = linspace(0, pi, N_ANGLES);
      proj_geom = astra_create_proj_geom('parallel3d', det_spacing, det_spacing, N, N, angles);
      vol_geom = astra_create_vol_geom(N, N, N);

      %% Calculate forward projection
      phantom_id = astra_mex_data3d('create', '-vol', vol_geom, phantom_);
      projection_id = astra_mex_data3d('create', '-sino', proj_geom);
      cfg = astra_struct('FP3D_CUDA');
      cfg.ProjectionDataId = projection_id;
      cfg.VolumeDataId = phantom_id;
      algorithm_id = astra_mex_algorithm('create', cfg);

      astra_mex_algorithm('run', algorithm_id);

      projection = astra_mex_data3d('get', projection_id);
      imshow(projection(:, :, N/2), []);

      %% Clean up
      astra_mex_data3d('delete', phantom_id, projection_id);
      astra_mex_algorithm('delete', algorithm_id);
