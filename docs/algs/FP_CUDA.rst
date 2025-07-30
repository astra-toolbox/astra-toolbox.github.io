FP_CUDA
=======

This is a GPU implementation of a simple forward projection algorithm for 2D data sets. It takes a volume as input, and returns the projection data.

Supported geometries: parallel, parallel_vec, fanflat, fanflat_vec.

Configuration options
---------------------
================================	========	==================================
name 					type 		description
================================	========	==================================
cfg.ProjectionDataId 			required 	`Data object ID <../concepts.html#data>`_ to store the result. The computed forward projection is added to this data.
cfg.VolumeDataId 			required 	`Volume data object ID <../concepts.html#data>`_
cfg.option.DetectorSuperSampling 	optional 	Specifies the amount of detector supersampling, i.e., how many rays are cast per detector.
cfg.option.GPUindex 			optional 	Specifies which GPU to use. Default = 0.
================================	========	==================================

Example
-------

.. code-block:: matlab

	%% create geometries
	proj_geom = astra_create_proj_geom('parallel', 1.0, 256, linspace2(0,pi,180));
	vol_geom = astra_create_vol_geom(256,256);

	%% store volume
	V = phantom(256);
	volume_id = astra_mex_data2d('create', '-vol', vol_geom, V);

	%% create forward projection
	sinogram_id = astra_mex_data2d('create', '-sino', proj_geom, 0);
	cfg = astra_struct('FP_CUDA');
	cfg.ProjectionDataId = sinogram_id;
	cfg.VolumeDataId = volume_id;
	fp_id = astra_mex_algorithm('create', cfg);
	astra_mex_algorithm('run', fp_id);
	sinogram = astra_mex_data2d('get', sinogram_id);
	imshow(sinogram, []);

	%% garbage disposal
	astra_mex_data2d('delete', sinogram_id, volume_id);
	astra_mex_algorithm('delete', fp_id);

This functionality can also be found in the astra function:

.. code-block:: matlab

  [sinogram_id, sinogram] = astra_create_sino_cuda(V, proj_geom, vol_geom);
  [sinogram_id, sinogram] = astra_create_sino_cuda(V_id, proj_geom, vol_geom);
