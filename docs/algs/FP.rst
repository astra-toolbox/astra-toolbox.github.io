FP
==

This is a CPU implementation of a simple forward projection algorithm for 2D data sets. It takes a projector and a volume as input, and returns the projection data.

Supported geometries: parallel, fanflat, fanflat_vec, matrix.

Configuration options
---------------------

========================== ======== =======================================================================================================
name                       type     description
========================== ======== =======================================================================================================
cfg.ProjectorId            required The astra_mex_projector ID of the projector.
cfg.ProjectionDataId       required The astra_mex_data2d ID of the projection data. The content of this data is overwritten.
cfg.VolumeDataId           required The astra_mex_data2d ID of the reconstruction data.
cfg.option.SinogramMaskId  optional If specified, the astra_mex_data2d ID of a projection-data-sized volume to be used as a mask.
cfg.option.VolumeMaskId    optional If specified, the astra_mex_data2d ID of a volume-data-sized volume to be used as a mask.
========================== ======== =======================================================================================================

Example
-------

.. code-block:: matlab

	%% create geometries and projector
	proj_geom = astra_create_proj_geom('parallel', 1.0, 256, linspace2(0,pi,180));
	vol_geom = astra_create_vol_geom(256,256);
	proj_id = astra_create_projector('linear', proj_geom, vol_geom);

	%% store volume
	V = phantom(256);
	volume_id = astra_mex_data2d('create', '-vol', vol_geom, V);

	%% create forward projection
	sinogram_id = astra_mex_data2d('create', '-sino', proj_geom, 0);
	cfg = astra_struct('FP');
	cfg.ProjectorId = proj_id;
	cfg.ProjectionDataId = sinogram_id;
	cfg.VolumeDataId = volume_id;
	fp_id = astra_mex_algorithm('create', cfg);
	astra_mex_algorithm('run', fp_id);
	sinogram = astra_mex_data2d('get', sinogram_id);
	imshow(sinogram, []);

	%% garbage disposal
	astra_mex_data2d('delete', sinogram_id, volume_id);
	astra_mex_projector('delete', proj_id);
	astra_mex_algorithm('delete', fp_id);

This functionality can also be found in the astra function:

.. code-block:: matlab

  [sinogram_id, sinogram] = astra_create_sino(V, proj_id);
  [sinogram_id, sinogram] = astra_create_sino(V_id, proj_id);
