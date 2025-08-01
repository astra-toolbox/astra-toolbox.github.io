FDK_CUDA
========

.. versionadded:: 2.2
   Add support for flexible filter type configuration. Before, only 'ram-lak' and 'sinorgam' filter types were supported.

This is a GPU implementation of the FDK algorithm for 3D circular cone beam data sets. It takes
projection data as input, and returns the reconstruction.

Supported geometries: cone, cone_vec.

**Note:** cone_vec geometries that significantly deviate from
a circular scanning geometry will likely lead to artifacts in the reconstruction.

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

  * - *option.FilterType*
    - | Type of projection filter. Options:
      | * 'none'
      | * 'ram-lak' (default)
      | * 'shepp-logan'
      | * 'cosine'
      | * 'hamming'
      | * 'hann'
      | * 'tukey'
      | * 'lanczos'
      | * 'triangular'
      | * 'gaussian'
      | * 'barlett-hann'
      | * 'blackman'
      | * 'nuttall'
      | * 'blackman-harris'
      | * 'blackman-nuttall'
      | * 'flat-top'
      | * 'kaiser'
      | * 'parzen'
      | * 'projection' (Fourier space filter, all projection directions share one filter)
      | * 'sinogram' (Fourier space filter, every projection direction has its own filter)
      | * 'rprojection' (real space filter, all projection directions share one filter)
      | * 'rsinogram' (real space filter, every projection direction has its own filter)

  * - *option.FilterParameter*
    - Parameter value for the 'tukey', 'gaussian', 'blackman' and 'kaiser'
      filter types.

  * - *option.FilterD*
    - "D" parameter value for 'shepp-logan', 'cosine', 'hamming' and 'hann'
      filter types.

  * - *option.FilterSinogramId*
    - The `data object ID <../concepts.html#data>`_ of the filter data for
      'projection', 'sinogram', 'rprojection' and 'rsinogram' filter types.

  * - *option.ShortScan*
    - If enabled, do Parker weighting to support non-360-degree data. This needs
      an angle range of at least 180 degrees plus twice the fan angle (default:
      false).

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
      N_ANGLES = 360
      det_spacing = 1.0
      angles = np.linspace(0, 2*np.pi, N_ANGLES)
      src_obj_dist = 500
      obj_det_dist = 0
      proj_geom = astra.create_proj_geom('cone', det_spacing, det_spacing, N, N, angles,
                                         src_obj_dist, obj_det_dist)
      vol_geom = astra.create_vol_geom(N, N, N)

      # Generate phantom image
      phantom_id, phantom = astra.data3d.shepp_logan(vol_geom)

      # Create forward projection
      sinogram_id, sinogram = astra.create_sino3d_gpu(phantom_id, proj_geom, vol_geom)

      # Reconstruct
      recon_id = astra.data3d.create('-vol', vol_geom)
      cfg = astra.astra_dict('FDK_CUDA')
      cfg['ProjectionDataId'] = sinogram_id
      cfg['ReconstructionDataId'] = recon_id
      algorithm_id = astra.algorithm.create(cfg)

      astra.algorithm.run(algorithm_id)

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
      N_ANGLES = 360;
      angles = linspace(0, 2*pi, N_ANGLES);
      src_obj_dist = 500;
      obj_det_dist = 0;
      proj_geom = astra_create_proj_geom('cone', det_spacing, det_spacing, N, N, angles, ...
                                         src_obj_dist, obj_det_dist);
      vol_geom = astra_create_vol_geom(N, N, N);

      %% Create forward projection
      [sinogram_id, sinogram] = astra_create_sino3d_cuda(phantom_, proj_geom, vol_geom);

      %% Reconstruct
      recon_id = astra_mex_data3d('create', '-vol', vol_geom);
      cfg = astra_struct('FDK_CUDA');
      cfg.ProjectionDataId = sinogram_id;
      cfg.ReconstructionDataId = recon_id;
      algorithm_id = astra_mex_algorithm('create', cfg);

      astra_mex_algorithm('run', algorithm_id);

      reconstruction = astra_mex_data3d('get', recon_id);
      imshow(reconstruction(:, :, N/2), []);

      %% Clean up
      astra_mex_data3d('delete', sinogram_id, recon_id);
      astra_mex_algorithm('delete', algorithm_id);
