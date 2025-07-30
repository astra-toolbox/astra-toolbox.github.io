Toolbox concepts
================

Geometry
--------

Most of the operations supported by the ASTRA Toolbox require you to specify the
geometry of the (real or virtual) experimental setup you have used or want to
simulate. This involves two parts: a volume geometry, and a projection geometry.

The volume geometry specifies where in 2D or 3D space the sample or phantom is
located. It takes the form of a 2D rectangle or 3D box, and is usually centered
around the origin.

The projection geometry specifies the location and trajectory
of the ray source and detector. The available types of projection geometries are
2D parallel beam, 2D fan beam, 3D parallel beam and 3D cone beam.

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      n_rows = 256
      n_cols = 256
      volume_geometry = astra.create_vol_geom(n_rows, n_cols)

      angles = np.linspace(0, np.pi, 180)
      detector_pixel_count = 256
      detector_spacing = 1.0
      projection_geometry = astra.create_proj_geom('parallel', detector_spacing, detector_pixel_count, angles)


  .. group-tab:: Matlab
    .. code-block:: matlab

      n_rows = 256;
      n_cols = 256;
      volume_geometry = astra_create_vol_geom(n_rows, n_cols);

      angles = linspace(0, pi, 180);
      detector_pixel_count = 256;
      detector_spacing = 1.0;
      projection_geometry = astra_create_proj_geom('parallel', detector_spacing, detector_pixel_count, angles);

Data
----

Data in ASTRA Toolbox is represented by objects separate from Python/Matlab, and
come in four varieties: 2D/3D volume data, and 2D/3D projection data. They can
be manipulated with `astra.data2d/astra_mex_data2d <data2d.html>`_ and
`astra.data3d/astra_mex_data3d <data2d.html>`_ commands. Each data object has a
unique ID that can be used to refer to it in algorithms or data manipulation
commands.

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      volume_data = np.random.rand(256, 256)
      projection_data = np.random.rand(180, 256)

      # Create ASTRA data object from NumPy arrays
      volume_data_id = astra.data2d.create('-vol', volume_geometry, data)
      projection_data_id = astra.data2d.create('-sino', volume_geometry, data)

      # Delete ASTRA data objects using their unique IDs
      astra.data2d.delete(volume_data_id)
      astra.data2d.delete(projection_data_id)

  .. group-tab:: Matlab
    .. code-block:: matlab

      volume_data = rand(256, 256);
      projection_data = rand(180, 256);

      % Create ASTRA data object from Matlab arrays
      volume_data_id = astra_mex_data2d('create', '-vol', volume_geometry, volume_data);
      projection_data_id = astra_mex_data2d('create', '-sino', projection_geometry, projection_data);

      % Delete ASTRA data objects using their unique IDs
      astra_mex_data2d('delete', volume_data_id);
      astra_mex_data2d('delete', projection_data_id);


Algorithms
----------

To operate on data objects, the ASTRA Toolbox defines `a number of algorithms
<algs/index.html>`_. These include the basic forward and backward projection
operations, and also various reconstruction algorithms.

The most flexible way to apply an algorithm to data is to create an algorithm
configuration, initialize an algorithm object, and call ``run`` command. There
result can then be retrieved from the corresponding ASTRA data object using
``get`` command.

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      config = astra.astra_dict('FP_CUDA')
      config['ProjectionDataId'] = projection_data_id
      config['VolumeDataId'] = volume_data_id

      algorithm_id = astra.algorithm.create(config)

      astra.algorithm.run(algorithm_id)

      sinogram = astra.data2d.get(projection_data_id)

  .. group-tab:: Matlab
    .. code-block:: matlab

	  config = astra_struct('FP_CUDA');
	  config.ProjectionDataId = projection_data_id;
	  config.VolumeDataId = volume_data_id;

	  algorithm_id = astra_mex_algorithm('create', config);

	  astra_mex_algorithm('run', algorithm_id);

	  sinogram = astra_mex_data2d('get', projection_data_id);


Each ASTRA algorithm object has a unique ID, which can be used to clean it up
after use, freeing the resources. Note that data objects are stored in RAM, but
algorithm objects may additionally reserve GPU memory.

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.algorithm.delete(algorithm_id)

  .. group-tab:: Matlab
    .. code-block:: matlab

	  astra_mex_algorithm('delete', algorithm_id);
