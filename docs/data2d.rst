2D Data Objects
===============

astra_mex_data2d
----------------

astra_mex_data2d is used to manage 2D data objects. It is a wrapper around the MEX file astra_mex_data2d_c.

2D data objects come in two varieties: volume data and projection data (sinograms).

astra_mex_data2d contains the following commands.

*    create
*    get
*    get_single
*    set / store
*    get_geometry
*    change_geometry
*    delete
*    clear
*    info

**create**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id = astra.data2d.create('-vol', vol_geom)
      id = astra.data2d.create('-vol', vol_geom, initializer)
  .. group-tab:: Matlab
    .. code-block:: matlab

      id = astra_mex_data2d('create', '-vol', vol_geom);
      id = astra_mex_data2d('create', '-vol', vol_geom, initializer);

This creates an initialized 2D volume data object for the geometry vol_geom.

Initializer may be:

*    a scalar: the object is initialized with this constant value.
*    a matrix: the object is initialized with the contents of this matrix. The matrix must be of size (y,x) as defined in the volume geometry. It must be of class single, double or logical.

If an initializer is not present, the volume is initialized to zero.

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id = astra.data2d.create('-sino', proj_geom)
      id = astra.data2d.create('-sino', proj_geom, initializer)

  .. group-tab:: Matlab
    .. code-block:: matlab

      id = astra_mex_data2d('create', '-sino', proj_geom);
      id = astra_mex_data2d('create', '-sino', proj_geom, initializer);

This creates an initialized 2D projection data object for the geometry proj_geom.

Initializer may be:

*    a scalar: the object is initialized with this constant value.
*    a matrix: the object is initialized with the contents of this matrix. The matrix must be of size (angles,u), where u is the number of detector pixels as defined in the projection geometry. It must be of class single, double or logical.

If an initializer is not present, the volume is initialized to zero.

**get**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      A = astra.data2d.get(id)

    This fetches the data object as a 2D array with dtype float32.

  .. group-tab:: Matlab
    .. code-block:: matlab

      A = astra_mex_data2d('get', id);

    This fetches the data object as a 2D matrix of class double.

**get_single**

.. tabs::
  .. group-tab:: Python

      N/A

  .. group-tab:: Matlab
    .. code-block:: matlab

      A = astra_mex_data2d('get_single', id);

    This fetches the data object as a 2D matrix of class single.

**get_shared**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      A = astra.data2d.get_shared(id)

    This fetches the data object as a 2D numpy array sharing the memory space
    of the ASTRA Toolbox. Do not delete the data object while memory is being
    shared this way.

  .. group-tab:: Matlab

    N/A

**set / store**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.data2d.store(id, A)


  .. group-tab:: Matlab
    .. code-block:: matlab

      astra_mex_data2d('set', id, A)
      astra_mex_data2d('store', id, A)

This stores the matrix A in the data object. The dimensions of A
must be the same as the existing data object.

Set and store are synonyms.

**get_geometry**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      geom = astra.data2d.get_geometry(id)

  .. group-tab:: Matlab
    .. code-block:: matlab

      geom = astra_mex_data2d('get_geometry', id);

This gets the (volume or projection) geometry attached to the object.

NB: This is not fully implemented yet and the return value may not accurately represent the geometry.

**change_geometry**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.data2d.change_geometry(id, geom)

  .. group-tab:: Matlab
    .. code-block:: matlab

      astra_mex_data2d('change_geometry', id, geom);

This changes the (volume or projection) geometry attached to the object.
It cannot change the dimensions of the data object. This can be used
to change the pixel dimensions or projection angles, for example.

**delete**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.data2d.delete(id)

  .. group-tab:: Matlab
    .. code-block:: matlab

      astra_mex_data2d('delete', id);

Free the memory of a data object.

**clear**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.data2d.clear()

  .. group-tab:: Matlab
    .. code-block:: matlab

      astra_mex_data2d('clear');

Free all data objects.

**info**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.data2d.info()

  .. group-tab:: Matlab
    .. code-block:: matlab

      astra_mex_data2d('info')

Print basic information about all allocated data objects.
