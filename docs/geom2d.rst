2D Geometries
=============

Volume Geometries
-----------------

Create a 2D volume geometry:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      vol_geom = astra.create_vol_geom(n_rows_and_cols)
      vol_geom = astra.create_vol_geom((n_rows, n_cols))
      vol_geom = astra.create_vol_geom(n_rows, n_cols)
      vol_geom = astra.create_vol_geom(n_rows, n_cols, min_x, max_x, min_y, max_y)

  .. group-tab:: Matlab
    .. code-block:: matlab

      vol_geom = astra_create_vol_geom(n_rows_and_cols);
      vol_geom = astra_create_vol_geom([n_rows n_cols]);
      vol_geom = astra_create_vol_geom(n_rows, n_cols);
      vol_geom = astra_create_vol_geom(n_rows, n_cols, min_x, max_x, min_y, max_y);

In the first form, the volume contains an equal number of rows and columns. In the first, second and third forms, volume pixels are squares with sides of unit length, and the volume is centered around the origin. In the fourth, longer form, the extents of the volume can be specified arbitrarily (note that rows are oriented along Y axis, and columns along X axis). The long form corresponding to the default short form is:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      vol_geom = astra.create_vol_geom(y, x, -x/2, x/2, -y/2, y/2)

  .. group-tab:: Matlab
    .. code-block:: matlab

      vol_geom = astra_create_vol_geom(y, x, -x/2, x/2, -y/2, y/2);

Note: For usage with GPU code, the volume must be centered around the origin and pixels must be square. This is not always explicitly checked in all functions, so not following these requirements may have unpredictable results.


Projection Geometries
---------------------

parallel
~~~~~~~~

Create a 2D parallel beam geometry:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_geom = astra.create_proj_geom('parallel', det_spacing, det_count, angles)

  .. group-tab:: Matlab
    .. code-block:: matlab

      proj_geom = astra_create_proj_geom('parallel', det_spacing, det_count, angles);

* det_spacing: distance between the centers of two adjacent detector pixels
* det_count: number of detector pixels in a single projection
* angles: projection angles in radians


fanflat
~~~~~~~

Create a 2D flat fan beam geometry:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_geom = astra.create_proj_geom('fanflat', det_spacing, det_count, angles, source_origin, origin_det)

  .. group-tab:: Matlab
    .. code-block:: matlab

      proj_geom = astra_create_proj_geom('fanflat', det_spacing, det_count, angles, source_origin, origin_det);

* det_spacing: distance between the centers of two adjacent detector pixels
* det_count: number of detector pixels in a single projection
* angles: projection angles in radians
* source_origin: distance between the source and the center of rotation
* origin_det: distance between the center of rotation and the detector array


parallel_vec
~~~~~~~~~~~~

Create a 2D parallel beam geometry specified by 2D vectors:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_geom = astra.create_proj_geom('parallel_vec', det_count, vectors)

  .. group-tab:: Matlab
    .. code-block:: matlab

      proj_geom = astra_create_proj_geom('parallel_vec', det_count, vectors);

* det_count: number of detectors in a single projection
* vectors: a matrix containing the actual geometry.
  Each row of vectors corresponds to a single projection, and consists of:
  ( rayX, rayY, dX, dY, uX, uY )
* ray : the ray direction
* d : the center of the detector
* u : the vector between the centers of detector pixels 0 and 1

To illustrate this, here is a script to convert a single projection in a projection geometry of
type "parallel" into such a 6-element row:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      # ray direction
      vectors[i,0] = numpy.sin(proj_geom['ProjectionAngles'][i])
      vectors[i,1] = -numpy.cos(proj_geom['ProjectionAngles'][i])

      # center of detector
      vectors[i,2] = 0
      vectors[i,3] = 0

      # vector from detector pixel 0 to 1
      vectors[i,4] = numpy.cos(proj_geom['ProjectionAngles'][i]) * proj_geom['DetectorWidth']
      vectors[i,5] = numpy.sin(proj_geom['ProjectionAngles'][i]) * proj_geom['DetectorWidth']

  .. group-tab:: Matlab
    .. code-block:: matlab

      % source
      vectors(i,1) = sin(proj_geom.ProjectionAngles(i));
      vectors(i,2) = -cos(proj_geom.ProjectionAngles(i));

      % center of detector
      vectors(i,3) = 0;
      vectors(i,4) = 0;

      % vector from detector pixel 0 to 1
      vectors(i,5) = cos(proj_geom.ProjectionAngles(i)) * proj_geom.DetectorWidth;
      vectors(i,6) = sin(proj_geom.ProjectionAngles(i)) * proj_geom.DetectorWidth;

This conversion is also available as a function in the toolbox:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_geom_vec = astra.geom_2vec(proj_geom)

  .. group-tab:: Matlab
    .. code-block:: matlab

      proj_geom_vec = astra_geom_2vec(proj_geom);


fanflat_vec
~~~~~~~~~~~

Create a 2D flat fan beam geometry specified by 2D vectors:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_geom = astra.create_proj_geom('fanflat_vec', det_count, vectors)

  .. group-tab:: Matlab
    .. code-block:: matlab

      proj_geom = astra_create_proj_geom('fanflat_vec', det_count, vectors);

* det_count: number of detectors in a single projection
* vectors: a matrix containing the actual geometry.
  Each row of vectors corresponds to a single projection, and consists of:
  ( srcX, srcY, dX, dY, uX, uY )
* src : the ray source
* d : the center of the detector
* u : the vector between the centers of detector pixels 0 and 1

To illustrate, this is a script to convert a single projection in a projection geometry of type "fanflat" into such a 6-element row:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      # source
      vectors[i,0] = numpy.sin(proj_geom['ProjectionAngles'][i]) * proj_geom['DistanceOriginSource']
      vectors[i,1] = -numpy.cos(proj_geom['ProjectionAngles'][i]) * proj_geom['DistanceOriginSource']

      # center of detector
      vectors[i,2] = -numpy.sin(proj_geom['ProjectionAngles'][i]) * proj_geom['DistanceOriginDetector']
      vectors[i,3] = numpy.cos(proj_geom['ProjectionAngles'][i]) * proj_geom['DistanceOriginDetector']

      # vector from detector pixel 0 to 1
      vectors[i,4] = numpy.cos(proj_geom['ProjectionAngles'][i]) * proj_geom['DetectorWidth']
      vectors[i,5] = numpy.sin(proj_geom['ProjectionAngles'][i]) * proj_geom['DetectorWidth']

  .. group-tab:: Matlab
    .. code-block:: matlab

      % source
      vectors(i,1) = sin(proj_geom.ProjectionAngles(i)) * proj_geom.DistanceOriginSource;
      vectors(i,2) = -cos(proj_geom.ProjectionAngles(i)) * proj_geom.DistanceOriginSource;

      % center of detector
      vectors(i,3) = -sin(proj_geom.ProjectionAngles(i)) * proj_geom.DistanceOriginDetector;
      vectors(i,4) = cos(proj_geom.ProjectionAngles(i)) * proj_geom.DistanceOriginDetector;

      % vector from detector pixel 0 to 1
      vectors(i,5) = cos(proj_geom.ProjectionAngles(i)) * proj_geom.DetectorWidth;
      vectors(i,6) = sin(proj_geom.ProjectionAngles(i)) * proj_geom.DetectorWidth;

This conversion is also available as a function in the toolbox:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_geom_vec = astra.geom_2vec(proj_geom)

  .. group-tab:: Matlab
    .. code-block:: matlab

      proj_geom_vec = astra_geom_2vec(proj_geom);


sparse_matrix
~~~~~~~~~~~~~

Create a 2D projection geometry defined by its system matrix:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_geom = astra.create_proj_geom('sparse_matrix', det_width, det_count, angles, matrix_id)

  .. group-tab:: Matlab
    .. code-block:: matlab

      proj_geom = astra_create_proj_geom('sparse_matrix', det_width, det_count, angles, matrix_id);

* det_width: unused, but has to be present (for compatibility reasons)
* det_count: number of detectors in a single projection
* angles: a vector, the length of which is the number of projections. The contents are unused.
* matrix_id: a `astra.matrix/astra_mex_matrix <misc.html#projection-matrix-objects>`_ object ID of a sparse matrix of the right dimensions.

The matrix is an ID returned by

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      matrix_id = astra.matrix.create(scipy_sparse_csr_matrix)

  .. group-tab:: Matlab
    .. code-block:: matlab

      matrix_id = astra_mex_matrix('create', matlab_sparse_matrix);

The sparse matrix must be of size (det_count * numel(angles), x*y), where (x,y) is the size
of the volume geometry to be used.

The rows of the sparse matrix are ordered by projection: The first row of the matrix
corresponds to the first detector pixel of the first projection, and the second row of the
matrix corresponds to the second detector pixel of the first projection.

The columns of the sparse matrix are ordered by row: The first column of the matrix
corresponds to pixel (0,0) in Python (which is (1,1) in Matlab) and the second
column to pixel (0,1) in Python (which is (1,2) in Matlab) in the volume.
