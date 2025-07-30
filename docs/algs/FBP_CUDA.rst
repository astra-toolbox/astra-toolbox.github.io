FBP_CUDA
========

This is a GPU implementation of the Filtered Backprojection (FBP) algorithm for 2D data sets. It takes projection data as input, and returns the reconstruction.

Supported geometries: parallel, fanflat

Configuration options
---------------------
+-------------------------------+----------+---------------------------------------------------------+
| name                          | type     | description                                             |
+===============================+==========+=========================================================+
| cfg.ProjectionDataId          | required | `Projection data object ID <../concepts.html#data>`_    |
+-------------------------------+----------+---------------------------------------------------------+
| cfg.ReconstructionDataId      | required | `ID of data object <../concepts.html#data>`_ to store   |
|                               |          | the result.                                             |
|                               |          |                                                         |
|                               |          | The content of this data is overwritten.                |
+-------------------------------+----------+---------------------------------------------------------+
| cfg.FilterType                | optional | | Type of projection filter. Options:                   |
|                               |          | | * 'none'                                              |
|                               |          | | * 'ram-lak' (default)                                 |
|                               |          | | * 'shepp-logan'                                       |
|                               |          | | * 'cosine'                                            |
|                               |          | | * 'hamming'                                           |
|                               |          | | * 'hann'                                              |
|                               |          | | * 'tukey'                                             |
|                               |          | | * 'lanczos'                                           |
|                               |          | | * 'triangular'                                        |
|                               |          | | * 'gaussian'                                          |
|                               |          | | * 'barlett-hann'                                      |
|                               |          | | * 'blackman'                                          |
|                               |          | | * 'nuttall'                                           |
|                               |          | | * 'blackman-harris'                                   |
|                               |          | | * 'blackman-nuttall'                                  |
|                               |          | | * 'flat-top'                                          |
|                               |          | | * 'kaiser'                                            |
|                               |          | | * 'parzen'                                            |
|                               |          | | * 'projection' (Fourier space filter, all projection  |
|                               |          | | directions share one filter)                          |
|                               |          | | * 'sinogram' (Fourier space filter, every projection  |
|                               |          | | direction has its own filter)                         |
|                               |          | | * 'rprojection' (real space filter, all projection    |
|                               |          | | directions share one filter)                          |
|                               |          | | * 'rsinogram' (real space filter, every projection    |
|                               |          | | direction has its own filter)                         |
+-------------------------------+----------+---------------------------------------------------------+
| cfg.FilterSinogramId          | optional | The `data object ID <../concepts.html#data>`_ of the    |
|                               |          |                                                         |
|                               |          | filter data for 'projection', 'sinogram', 'rprojection' |
|                               |          |                                                         |
|                               |          | and 'rsinogram' filter types.                           |
+-------------------------------+----------+---------------------------------------------------------+
| cfg.FilterParameter           | optional | Parameter value for the 'tukey', 'gaussian',            |
|                               |          |                                                         |
|                               |          | 'blackman' and 'kaiser' filter types.                   |
+-------------------------------+----------+---------------------------------------------------------+
| cfg.FilterD                   | optional | "D" parameter value for 'shepp-logan', 'cosine',        |
|                               |          |                                                         |
|                               |          | 'hamming' and 'hann'  filter types.                     |
+-------------------------------+----------+---------------------------------------------------------+
| cfg.option.GPUindex           | optional | Specifies which GPU to use. Default = 0.                |
+-------------------------------+----------+---------------------------------------------------------+
| cfg.option.PixelSuperSampling | optional | Specifies the amount of pixel supersampling, i.e.,      |
|                               |          |                                                         |
|                               |          | how many (one dimension) subpixels are generated from   |
|                               |          |                                                         |
|                               |          | a single parent pixel.                                  |
+-------------------------------+----------+---------------------------------------------------------+
| cfg.option.ShortScan          | optional | Only for use with the fanflat geometry. If enabled,     |
|                               |          |                                                         |
|                               |          | do Parker weighting to support non-360-degree data.     |
|                               |          |                                                         |
|                               |          | This needs an angle range of at least 180 degrees       |
|                               |          |                                                         |
|                               |          | plus twice the fan angle. Defaults to no.               |
+-------------------------------+----------+---------------------------------------------------------+

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
      cfg = astra.astra_dict('FBP_CUDA')
      cfg['ProjectorId'] = proj_id
      cfg['ProjectionDataId'] = sinogram_id
      cfg['ReconstructionDataId'] = recon_id
      fbp_id = astra.algorithm.create(cfg)
      astra.algorithm.run(fbp_id)
      V = astra.data2d.get(recon_id)
      plt.gray()
      plt.imshow(V)
      plt.show()

      # garbage disposal
      astra.data2d.delete([sinogram_id, recon_id, V_exact_id])
      astra.projector.delete(proj_id)
      astra.algorithm.delete(fbp_id)


  .. group-tab:: Matlab
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
	cfg = astra_struct('FBP_CUDA');
	cfg.ProjectionDataId = sinogram_id;
	cfg.ReconstructionDataId = recon_id;
	fbp_id = astra_mex_algorithm('create', cfg);
	astra_mex_algorithm('run', fbp_id);
	V = astra_mex_data2d('get', recon_id);
	imshow(V, []);

	%% garbage disposal
	astra_mex_data2d('delete', sinogram_id, recon_id);
	astra_mex_algorithm('delete', fbp_id);

