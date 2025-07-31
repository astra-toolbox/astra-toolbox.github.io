SIRT_CUDA
=========

This is a GPU implementation of the Simultaneous Iterative Reconstruction Technique (SIRT) for 2D data sets. It takes projection data and an initial reconstruction as input, and returns the reconstruction after a specified number of SIRT iterations.

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
      import numpy

      # create geometries and projector
      proj_geom = astra.create_proj_geom('parallel', 1.0, 256, numpy.linspace(0, numpy.pi, 180, endpoint=False))
      vol_geom = astra.create_vol_geom(256,256)
      proj_id = astra.create_projector('cuda', proj_geom, vol_geom)

      # generate phantom image
      V_exact_id, V_exact = astra.data2d.shepp_logan(vol_geom)

      # create forward projection
      sinogram_id, sinogram = astra.create_sino(V_exact, proj_id)

      # reconstruct
      recon_id = astra.data2d.create('-vol', vol_geom, 0)
      cfg = astra.astra_dict('SIRT_CUDA')
      cfg['ProjectorId'] = proj_id
      cfg['ProjectionDataId'] = sinogram_id
      cfg['ReconstructionDataId'] = recon_id
      cfg['option'] = { 'MinConstraint': 0, 'MaxConstraint': 1 }
      sirt_id = astra.algorithm.create(cfg)
      astra.algorithm.run(sirt_id, 100)
      V = astra.data2d.get(recon_id)
      plt.gray()
      plt.imshow(V)
      plt.show()

      # garbage disposal
      astra.data2d.delete([sinogram_id, recon_id, V_exact_id])
      astra.projector.delete(proj_id)
      astra.algorithm.delete(sirt_id)


  .. group-tab:: MATLAB
    .. code-block:: matlab

	%% create phantom
	V_exact = phantom(256);

	%% create geometries
	proj_geom = astra_create_proj_geom('parallel', 1.0, 256, linspace2(0,pi,180));
	vol_geom = astra_create_vol_geom(256,256);

	%% create forward projection
	[sinogram_id, sinogram] = astra_create_sino_cuda(V_exact, proj_geom, vol_geom);

	%% reconstruct
	recon_id = astra_mex_data2d('create', '-vol', vol_geom, 0);
	cfg = astra_struct('SIRT_CUDA');
	cfg.ProjectionDataId = sinogram_id;
	cfg.ReconstructionDataId = recon_id;
	cfg.option.MinConstraint = 0;
	cfg.option.MaxConstraint = 255;
	sirt_id = astra_mex_algorithm('create', cfg);
	astra_mex_algorithm('iterate', sirt_id, 100);
	V = astra_mex_data2d('get', recon_id);
	imshow(V, []);

	%% garbage disposal
	astra_mex_data2d('delete', sinogram_id, recon_id);
	astra_mex_algorithm('delete', sirt_id);

Extra features
--------------

SIRT_CUDA supports astra.algorithm.get_res_norm() / astra_mex_algorithm('get_res_norm') to get the
2-norm of the difference between the projection data and the projection of the reconstruction. (The
square root of the sum of squares of differences.)
