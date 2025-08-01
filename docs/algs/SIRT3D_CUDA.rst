SIRT3D_CUDA
===========

This is a GPU implementation of the SIRT algorithm for 3D data sets.
It takes projection data and an initial reconstruction as input, and
returns the reconstruction after a specified number of SIRT iterations.

Supported geometries: all 3D geometries.

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
      content of this data is used as the initial reconstruction.

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

  * - *option.DetectorSuperSampling*
    - During forward projection, each detector pixel will be subdivided by this
      factor along each dimension. This should only be used if detector pixels
      are larger than the voxels in the volume (default: 1).

  * - *option.VoxelSuperSampling*
    - During backprojection, each voxel in the volume will be subdivided by this
      factor along each dimension. This should only be used if voxels in the
      volume are larger than the detector pixels (default: 1).

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

      # Reconstruct
      recon_id = astra.data3d.create('-vol', vol_geom)
      cfg = astra.astra_dict('SIRT3D_CUDA')
      cfg['ProjectionDataId'] = sinogram_id
      cfg['ReconstructionDataId'] = recon_id
      cfg['option'] = {'MinConstraint': 0.0}
      algorithm_id = astra.algorithm.create(cfg)

      astra.algorithm.run(algorithm_id, iterations=100)

      reconstruction = astra.data3d.get(recon_id)
      plt.imshow(reconstruction[N//2], cmap='gray')

      # Clean up
      astra.data3d.delete([sinogram_id, recon_id, phantom_id])
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

      %% Reconstruct
      recon_id = astra_mex_data3d('create', '-vol', vol_geom);
      cfg = astra_struct('SIRT3D_CUDA');
      cfg.ProjectionDataId = sinogram_id;
      cfg.ReconstructionDataId = recon_id;
      cfg.option.MinConstraint = 0.0;
      algorithm_id = astra_mex_algorithm('create', cfg);

      astra_mex_algorithm('iterate', algorithm_id, 100);

      reconstruction = astra_mex_data3d('get', recon_id);
      imshow(reconstruction(:, :, N/2), []);

      %% Clean up
      astra_mex_data3d('delete', sinogram_id, recon_id);
      astra_mex_algorithm('delete', algorithm_id);


Extra features
--------------

SIRT3D_CUDA supports ``astra.algorithm.get_res_norm()`` /
``astra_mex_algorithm('get_res_norm')`` command to get the 2-norm of the
residual for the reconstruction (the square root of the sum of squares of
differences between the input and the projection of the result).
