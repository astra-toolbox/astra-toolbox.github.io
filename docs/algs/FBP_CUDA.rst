FBP_CUDA
========

This is a CPU implementation of the Filtered Backprojection (FBP) algorithm for 2D data sets. It takes projection data as input, and returns the reconstruction.

Supported geometries: parallel, fanflat

Configuration options
---------------------
==============================	========	==
name 				type 		description
==============================	========	==
cfg.ProjectionDataId 		required 	The astra_mex_data2d ID of the projection data
cfg.ReconstructionDataId 	required 	The astra_mex_data2d ID of the reconstruction data. The content of this is overwritten.
cfg.FilterType 			optional 	Type of projection filter. Options: 'ram-lak' (default), 'shepp-logan', 'cosine', 'hamming', 'hann', 'none', 'tukey', 'lanczos', 'triangular', 'gaussian', 'barlett-hann', 'blackman', 'nuttall', 'blackman-harris', 'blackman-nuttall', 'flat-top', 'kaiser', 'parzen', 'projection', 'sinogram', 'rprojection', 'rsinogram'.
cfg.FilterSinogramId 		optional 	Only for some FilterTypes.
cfg.FilterParameter 		optional 	Only for some FilterTypes.
cfg.FilterD 			optional 	Only for some FilterTypes.
cfg.option.GPUindex 		optional 	Specifies which GPU to use. Default = 0.
cfg.option.PixelSuperSampling 	optional 	Specifies the amount of pixel supersampling, i.e., how many (one dimension) subpixels are generated from a single parent pixel.
cfg.option.ShortScan 		optional 	Only for use with the fanflat geometry. If enabled, do Parker weighting to support non-360-degree data. This needs an angle range of at least 180 degrees plus twice the fan angle. Defaults to no.
==============================	========	==

Example
-------

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

