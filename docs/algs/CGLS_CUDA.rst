CGLS_CUDA
=========

This is a GPU implementation of the Conjugate Gradient Least Squares (CGLS) algorithm for 2D data sets. It takes projection data and an initial reconstruction as input, and returns the reconstruction after a specified number of iterations.

The internal state of the CGLS algorithm is reset every time
``astra.algorithm.run``/``astra_mex_algorithm('iterate')`` is called. This
implies that running CGLS for N iterations and then running it for another N
iterations may yield different results from running it 2N iterations at once.

Supported geometries: parallel, parallel_vec, fanflat, fanflat_vec.

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

  * - *option.ReconstructionMaskId*
    - If specified, `data object ID <../concepts.html#data>`_ of a
      volume-data-sized volume to be used as a `mask <../misc.html#masks>`_.

  * - *option.DetectorSuperSampling*
    - During forward projection, each detector element will be subdivided by
      this factor along each dimension. This should only be used if detector
      elements are larger than the pixels in the volume (default: 1).

  * - *option.PixelSuperSampling*
    - During backprojection, each pixel in the volume will be subdivided by this
      factor along each dimension. This should only be used if pixels in the
      volume are larger than the detector elements (default: 1).

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
      recon_id = astra.data2d.create('-vol', vol_geom)
      cfg = astra.astra_dict('CGLS_CUDA')
      cfg['ProjectionDataId'] = sinogram_id
      cfg['ReconstructionDataId'] = recon_id
      algorithm_id = astra.algorithm.create(cfg)

      astra.algorithm.run(algorithm_id, iterations=100)

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
      recon_id = astra_mex_data2d('create', '-vol', vol_geom);
      cfg = astra_struct('CGLS_CUDA');
      cfg.ProjectionDataId = sinogram_id;
      cfg.ReconstructionDataId = recon_id;
      algorithm_id = astra_mex_algorithm('create', cfg);

      astra_mex_algorithm('iterate', algorithm_id, 100);

      reconstruction = astra_mex_data2d('get', recon_id);
      imshow(reconstruction, []);

      %% Clean up
      astra_mex_data2d('delete', sinogram_id, recon_id);
      astra_mex_projector('delete', projector_id);
      astra_mex_algorithm('delete', algorithm_id);


Extra features
--------------

CGLS_CUDA supports astra.algorithm.get_res_norm() / astra_mex_algorithm('get_res_norm') to get the
2-norm of the difference between the projection data and the projection of the reconstruction. (The
square root of the sum of squares of differences.)
