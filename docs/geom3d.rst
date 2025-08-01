3D Geometries
=============

Volume geometries
-----------------

Create a 3D volume geometry:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

     vol_geom = astra.create_vol_geom([n_rows, n_cols, n_slices])
     vol_geom = astra.create_vol_geom(n_rows, n_cols, n_slices)

  .. group-tab:: MATLAB
    .. code-block:: matlab

     vol_geom = astra_create_vol_geom([n_rows, n_cols, n_slices]);
     vol_geom = astra_create_vol_geom(n_rows, n_cols, n_slices);

Specify the extent of the 3D volume (note that rows are oriented along the Y axis, columns along the X axis and slices along the Z axis):

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      vol_geom = astra.create_vol_geom(n_rows, n_cols, n_slices, min_x, max_x, min_y, max_y, min_z, max_z)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      vol_geom = astra_create_vol_geom(n_rows, n_cols, n_slices, min_x, max_x, min_y, max_y, min_z, max_z);

This can be used to control the voxel size, including specifying anisotropic voxels (note that the FDK algorithm does not currently support anisotropic voxels and will raise an exception).


Projection geometries
---------------------

parallel3d
~~~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_geom = astra.create_proj_geom('parallel3d', det_spacing_x, det_spacing_y, det_row_count, det_col_count, angles)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      proj_geom = astra_create_proj_geom('parallel3d', det_spacing_x, det_spacing_y, det_row_count, det_col_count, angles);

Create a 3D parallel beam geometry.

*    det_spacing_x: distance between the centers of two horizontally adjacent detector pixels
*    det_spacing_y: distance between the centers of two vertically adjacent detector pixels
*    det_row_count: number of detector rows in a single projection
*    det_col_count: number of detector columns in a single projection
*    angles: projection angles in radians


cone
~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_geom = astra.create_proj_geom('cone',  det_spacing_x, det_spacing_y, det_row_count, det_col_count, angles, source_origin, origin_det)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      proj_geom = astra_create_proj_geom('cone',  det_spacing_x, det_spacing_y, det_row_count, det_col_count, angles, source_origin, origin_det);

Create a 3D cone beam geometry.

*    det_spacing_x: distance between the centers of two horizontally adjacent detector pixels
*    det_spacing_y: distance between the centers of two vertically adjacent detector pixels
*    det_row_count: number of detector rows in a single projection
*    det_col_count: number of detector columns in a single projection
*    angles: projection angles in radians
*    source_origin: distance between the source and the center of rotation
*    origin_det: distance between the center of rotation and the detector array


parallel3d_vec
~~~~~~~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_geom = astra.create_proj_geom('parallel3d_vec',  det_row_count, det_col_count, vectors)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      proj_geom = astra_create_proj_geom('parallel3d_vec',  det_row_count, det_col_count, vectors);

Create a 3D parallel beam geometry specified by 3D vectors.

*    det_row_count: number of detector rows in a single projection
*    det_col_count: number of detector columns in a single projection
*    vectors: a matrix containing the actual geometry.

Each row of vectors corresponds to a single projection, and consists of:

.. code-block:: matlab

  ( rayX, rayY, rayZ, dX, dY, dZ, uX, uY, uZ, vX, vY, vZ )

* ray : the ray direction
* d   : the center of the detector
* u   : the vector from detector pixel (0,0) to (0,1)
* v   : the vector from detector pixel (0,0) to (1,0)

To illustrate this, here is a script to convert a single projection in a projection geometry of
type "parallel3d" into such a 12-element row:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      # ray direction
      vectors[i,0] = numpy.sin(proj_geom['ProjectionAngles'][i])
      vectors[i,1] = -numpy.cos(proj_geom['ProjectionAngles'][i])
      vectors[i,2] = 0

      # center of detector
      vectors[i,3] = 0
      vectors[i,4] = 0
      vectors[i,5] = 0

      # vector from detector pixel (0,0) to (0,1)
      vectors[i,6] = numpy.cos(proj_geom['ProjectionAngles'][i]) * proj_geom['DetectorSpacingX']
      vectors[i,7] = numpy.sin(proj_geom['ProjectionAngles'][i]) * proj_geom['DetectorSpacingX']
      vectors[i,8] = 0

      # vector from detector pixel (0,0) to (1,0)
      vectors[i, 9] = 0
      vectors[i,10] = 0
      vectors[i,11] = proj_geom['DetectorSpacingY']

  .. group-tab:: MATLAB
    .. code-block:: matlab

      % ray direction
      vectors(i,1) = sin(proj_geom.ProjectionAngles(i));
      vectors(i,2) = -cos(proj_geom.ProjectionAngles(i));
      vectors(i,3) = 0;

      % center of detector
      vectors(i,4) = 0;
      vectors(i,5) = 0;
      vectors(i,6) = 0;

      % vector from detector pixel (0,0) to (0,1)
      vectors(i,7) = cos(proj_geom.ProjectionAngles(i)) * proj_geom.DetectorSpacingX;
      vectors(i,8) = sin(proj_geom.ProjectionAngles(i)) * proj_geom.DetectorSpacingX;
      vectors(i,9) = 0;

      % vector from detector pixel (0,0) to (1,0)
      vectors(i,10) = 0;
      vectors(i,11) = 0;
      vectors(i,12) = proj_geom.DetectorSpacingY;

This conversion is also available as a function in the toolbox:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_geom_vec = astra.geom_2vec(proj_geom)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      proj_geom_vec = astra_geom_2vec(proj_geom);


cone_vec
~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_geom = astra.create_proj_geom('cone_vec',  det_row_count, det_col_count, vectors)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      proj_geom = astra_create_proj_geom('cone_vec',  det_row_count, det_col_count, vectors);

Create a 3D cone beam geometry specified by 3D vectors.

*    det_row_count: number of detector rows in a single projection
*    det_col_count: number of detector columns in a single projection
*    vectors: a matrix containing the actual geometry.

Each row of vectors corresponds to a single projection, and consists of:

.. code-block:: matlab

 ( srcX, srcY, srcZ, dX, dY, dZ, uX, uY, uZ, vX, vY, vZ )

* src : the ray source
* d   : the center of the detector
* u   : the vector from detector pixel (0,0) to (0,1)
* v   : the vector from detector pixel (0,0) to (1,0)

To illustrate this, here is a script to convert a single projection in a projection geometry of
type "cone" into such a 12-element row:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      # source
      vectors[i,0] = numpy.sin(proj_geom['ProjectionAngles'][i]) * proj_geom['DistanceOriginSource']
      vectors[i,1] = -numpy.cos(proj_geom['ProjectionAngles'][i]) * proj_geom['DistanceOriginSource']
      vectors[i,2] = 0

      # center of detector
      vectors[i,3] = -numpy.sin(proj_geom['ProjectionAngles'][i]) * proj_geom['DistanceOriginDetector']
      vectors[i,4] = numpy.cos(proj_geom['ProjectionAngles'][i]) * proj_geom['DistanceOriginDetector']
      vectors[i,5] = 0

      # vector from detector pixel (0,0) to (0,1)
      vectors[i,6] = numpy.cos(proj_geom['ProjectionAngles'][i]) * proj_geom['DetectorSpacingX']
      vectors[i,7] = numpy.sin(proj_geom['ProjectionAngles'][i]) * proj_geom['DetectorSpacingX']
      vectors[i,8] = 0

      # vector from detector pixel (0,0) to (1,0)
      vectors[i, 9] = 0
      vectors[i,10] = 0
      vectors[i,11] = proj_geom['DetectorSpacingY']

  .. group-tab:: MATLAB
    .. code-block:: matlab

      % source
      vectors(i,1) = sin(proj_geom.ProjectionAngles(i)) * proj_geom.DistanceOriginSource;
      vectors(i,2) = -cos(proj_geom.ProjectionAngles(i)) * proj_geom.DistanceOriginSource;
      vectors(i,3) = 0;

      % center of detector
      vectors(i,4) = -sin(proj_geom.ProjectionAngles(i)) * proj_geom.DistanceOriginDetector;
      vectors(i,5) = cos(proj_geom.ProjectionAngles(i)) * proj_geom.DistanceOriginDetector;
      vectors(i,6) = 0;

      % vector from detector pixel (0,0) to (0,1)
      vectors(i,7) = cos(proj_geom.ProjectionAngles(i)) * proj_geom.DetectorSpacingX;
      vectors(i,8) = sin(proj_geom.ProjectionAngles(i)) * proj_geom.DetectorSpacingX;
      vectors(i,9) = 0;

      % vector from detector pixel (0,0) to (1,0)
      vectors(i,10) = 0;
      vectors(i,11) = 0;
      vectors(i,12) = proj_geom.DetectorSpacingY;


cyl_cone_vec
~~~~~~~~~~~~

.. versionadded:: 2.4

.. caution:: This is an experimental feature, and the parameters or implementation may change in future releases.

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_geom = astra.create_proj_geom('cyl_cone_vec',  det_row_count, det_col_count, vectors)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      proj_geom = astra_create_proj_geom('cyl_cone_vec',  det_row_count, det_col_count, vectors);

Create a 3D cylindrical detector cone beam geometry specified by 3D vectors. U axis of the detector
will be curved.

*    det_row_count: number of detector rows in a single projection
*    det_col_count: number of detector columns in a single projection
*    vectors: a matrix containing the actual geometry.

Each row of vectors corresponds to a single projection, and consists of:

.. code-block:: matlab

 ( srcX, srcY, srcZ, dX, dY, dZ, uX, uY, uZ, vX, vY, vZ, R )

* src : the ray source
* d   : the center of the detector
* u   : the vector from detector pixel (0,0) to (0,1)
* v   : the vector from detector pixel (0,0) to (1,0)
* R   : curvature radius of the cylindrical detector (U axis will be curved)
