2D Data Objects
===============

Data objects come in two varieties: volume data and projection data
(sinograms), and can be manipulated using the following commands:

*    create
*    link (python)
*    get
*    get_shared (python)
*    get_single (matlab)
*    set / store
*    get_geometry
*    change_geometry
*    delete
*    clear
*    info
*    shepp_logan (python)

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

**link**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id = astra.data2d.link('-sino', proj_geom, array)
      id = astra.data2d.link('-vol', vol_geom, array)

  .. group-tab:: Matlab
    .. code-block:: matlab

      N/A

Creates an Astra data object that shares its memory with the specified numpy.ndarray. The ndarray
must be contiguous, have float32 dtype, and be of the right shape. Changes to the ndarray will be
visible to Astra, and vice versa. This increments the reference count of the underlying memory, so
it is safe to delete the linked ndarray while the Astra object still exists.

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

**get_shared**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      A = astra.data2d.get_shared(id)

    This fetches the data object as a 2D numpy array sharing its memory with the Astra object.
    Changes to the returned array will be visible to Astra, and vice versa. Deleting the Astra
    object while the resulting Python object still exists will lead to undefined behaviour and
    potentially memory corruption and crashes.

  .. group-tab:: Matlab

    N/A

**get_single**

.. tabs::
  .. group-tab:: Python

      N/A

  .. group-tab:: Matlab
    .. code-block:: matlab

      A = astra_mex_data2d('get_single', id);

    This fetches the data object as a 2D matrix of class single.

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

Set and store are synonyms in the Matlab interface.

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

**shepp_logan**

.. versionadded:: 2.2

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id, data = astra.data2d.shepp_logan(vol_geom, modified)

  .. group-tab:: Matlab
    .. code-block:: matlab

      N/A

Creates a Shepp-Logan transform. ``modified=True`` creates a phantom with improved contrast (default).
