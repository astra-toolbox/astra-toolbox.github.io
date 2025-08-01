FBP_CUDA
========

This is a GPU implementation of the Filtered Backprojection (FBP) algorithm for 2D data sets. It takes projection data as input, and returns the reconstruction.

Supported geometries: parallel, fanflat

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
    - Only for use with the fanflat geometry. If enabled, do Parker weighting to
      support non-360-degree data. This needs an angle range of at least 180
      degrees plus twice the fan angle (default: false).

  * - *option.PixelSuperSampling*
    - Each pixel in the volume will be subdivided by this factor along each
      dimension. This should only be used if pixels in the volume are larger
      than the detector elements (default: 1).

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

      # Create geometries and projector
      N = 256
      N_ANGLES = 180
      det_spacing = 1.0
      angles = np.linspace(0, np.pi, N_ANGLES)
      proj_geom = astra.create_proj_geom('parallel', det_spacing, N, angles)
      vol_geom = astra.create_vol_geom(N, N)
      projector_id = astra.create_projector('cuda', proj_geom, vol_geom)

      # Generate phantom image
      phantom_id, phantom = astra.data2d.shepp_logan(vol_geom)

      # Create forward projection
      sinogram_id, sinogram = astra.create_sino(phantom_id, projector_id)

      # Reconstruct
      recon_id = astra.data2d.create('-vol', vol_geom, data=1.0)
      cfg = astra.astra_dict('FBP_CUDA')
      cfg['ProjectionDataId'] = sinogram_id
      cfg['ReconstructionDataId'] = recon_id
      algorithm_id = astra.algorithm.create(cfg)

      astra.algorithm.run(algorithm_id)

      reconstruction = astra.data2d.get(recon_id)
      plt.imshow(reconstruction, cmap='gray')

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
      projector_id = astra_create_projector('cuda', proj_geom, vol_geom);

      %% Create forward projection
      [sinogram_id, sinogram] = astra_create_sino(phantom, projector_id);

      %% Reconstruct
      recon_id = astra_mex_data2d('create', '-vol', vol_geom, 1.0);
      cfg = astra_struct('FBP_CUDA');
      cfg.ProjectionDataId = sinogram_id;
      cfg.ReconstructionDataId = recon_id;
      algorithm_id = astra_mex_algorithm('create', cfg);

      astra_mex_algorithm('run', algorithm_id);

      reconstruction = astra_mex_data2d('get', recon_id);
      imshow(reconstruction, []);

      %% Clean up
      astra_mex_data2d('delete', sinogram_id, recon_id);
      astra_mex_projector('delete', projector_id);
      astra_mex_algorithm('delete', algorithm_id);
