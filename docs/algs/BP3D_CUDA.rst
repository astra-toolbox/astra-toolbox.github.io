BP3D_CUDA
=========

This is a GPU implementation of a simple backprojection algorithm for 3D data
sets. It takes projection data as input, and returns the backprojection of this
data.

Configuration options
---------------------

.. list-table::
  :header-rows: 1

  * - Name
    - Description

  * - ProjectionDataId
    - `Projection data object ID <../concepts.html#data>`_.

  * - ReconstructionDataId
    - `ID of data object <../concepts.html#data>`_ to store the result. The
      content of this data is overwritten.

  * - *option.VoxelSuperSampling*
    - Each voxel in the volume will be subdivided by this factor along each
      dimension. This should only be used if voxels in the volume are
      larger than the detector pixels (default: 1).

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

      # Create forward projection
      sinogram_id, sinogram = astra.create_sino3d_gpu(phantom_id, proj_geom, vol_geom)

      # Calculate backprojection
      backprojection_id = astra.data3d.create('-vol', vol_geom)
      cfg = astra.astra_dict('BP3D_CUDA')
      cfg['ProjectionDataId'] = sinogram_id
      cfg['ReconstructionDataId'] = backprojection_id
      algorithm_id = astra.algorithm.create(cfg)

      astra.algorithm.run(algorithm_id)

      backprojection = astra.data3d.get(backprojection_id)
      plt.imshow(backprojection[N//2], cmap='gray')

      # Clean up
      astra.data3d.delete([sinogram_id, backprojection_id, phantom_id])
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

      %% Create forward projection
      [sinogram_id, sinogram] = astra_create_sino3d_cuda(phantom_, proj_geom, vol_geom);

      %% Calculate backprojection
      backprojection_id = astra_mex_data3d('create', '-vol', vol_geom);
      cfg = astra_struct('BP3D_CUDA');
      cfg.ProjectionDataId = sinogram_id;
      cfg.ReconstructionDataId = backprojection_id;
      algorithm_id = astra_mex_algorithm('create', cfg);

      astra_mex_algorithm('run', algorithm_id);

      backprojection = astra_mex_data3d('get', backprojection_id);
      imshow(backprojection(:, :, N/2), []);

      %% Clean up
      astra_mex_data3d('delete', sinogram_id, backprojection_id);
      astra_mex_algorithm('delete', algorithm_id);
