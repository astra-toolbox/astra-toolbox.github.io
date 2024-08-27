3D Data Objects
===============

astra_mex_data3d
----------------

astra_mex_data3d is used to manage 3D data objects. It is a wrapper around the MEX file astra_mex_data3d_c.

3D data objects come in two varieties: volume data and projection data.

astra_mex_data3d has the following commands.

*    create
*    get
*    get_single (matlab)
*    set / store
*    dimensions
*    delete
*    clear
*    info
*    link
*    get_shared (python)

**create**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id = astra.data3d.create('-vol', vol_geom)
      id = astra.data3d.create('-vol', vol_geom, initializer)

  .. group-tab:: Matlab
    .. code-block:: matlab

      id = astra_mex_data3d('create', '-vol', vol_geom);
      id = astra_mex_data3d('create', '-vol', vol_geom, initializer);

This creates an initialized 3D volume data object for the geometry vol_geom.

Initializer may be:

*    a scalar: the object is initialized with this constant value.
*    a matrix: the object is initialized with the contents of this matrix. The matrix must be of size (n_slices, n_rows, n_cols) in Python, or (n_cols, n_rows, n_slices) in Matlab, as defined in the volume geometry. In Python, it must be convertible to dtype float32. In Matlab, it must be of class single, double or logical.

If an initializer is not present, the volume is initialized to zero.

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

     id = astra.data3d.create('-proj3d', proj_geom)
     id = astra.data3d.create('-proj3d', proj_geom, initializer)

  .. group-tab:: Matlab
    .. code-block:: matlab

     id = astra_mex_data3d('create', '-proj3d', proj_geom);
     id = astra_mex_data3d('create', '-proj3d', proj_geom, initializer);

This creates an initialized 3D projection data object for the geometry proj_geom.

Initializer may be:

*    a scalar: the object is initialized with this constant value.
*    a matrix: the object is initialized with the contents of this matrix. The matrix must be of size (det_row_count, n_angles, det_col_count) in Python, or (det_col_count, n_angles, det_row_count) in Matlab, as defined in the projection geometry. In Python, it must be convertible to dtype float32. In Matlab, it must be of class single, double or logical.

If an initializer is not present, the volume is initialized to zero.

**get**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      A = astra.data3d.get(id)

  .. group-tab:: Matlab
    .. code-block:: matlab

      A = astra_mex_data3d('get', id);

This fetches the data object as a 3D matrix. In Matlab, it will be of class double. In Python, of dtype float32.

**matlab get_single**

.. code-block:: matlab

  A = astra_mex_data3d('get_single', id);

This fetches the data object as a 3D matrix of class single.

**set / store**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.data3d.store(id, A)

  .. group-tab:: Matlab
    .. code-block:: matlab

      astra_mex_data3d('set', id, A);
      astra_mex_data3d('store', id, A);

This stores the matrix A in the data object. The dimensions of A
must be the same as when used as an initializer in astra_mex_data3d('create').

Set and store are synonyms in the Matlab interface.

**dimensions**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      s = astra.data3d.dimensions(id)

  .. group-tab:: Matlab
    .. code-block:: matlab

      s = astra_mex_data3d('dimensions', id);

Get the dimensions of a data object.

**delete**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.data3d.delete(id)
      astra.data3d.delete([id1, id2, ...])

  .. group-tab:: Matlab
    .. code-block:: matlab

      astra_mex_data3d('delete', id);

Free the memory of a data object.

**clear**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.data3d.clear()

  .. group-tab:: Matlab
    .. code-block:: matlab

      astra_mex_data3d('clear');

Free all data objects.

**info**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.data3d.info()

  .. group-tab:: Matlab
    .. code-block:: matlab

      astra_mex_data3d('info')

Print basic information about all allocated data objects.

**matlab link**

.. code-block:: matlab

 id = astra_mex_data3d_c('link', '-vol', vol_geom, array, readonly, Z);
 id = astra_mex_data3d_c('link', '-proj3d', proj_geom, array, readonly, Z);

NB: This must be called on astra_mex_data3d_c, and does not work properly
when using the wrapper astra_mex_data3d.

This creates a data object that directly uses a matlab array as storage
instead of allocating its own memory. The array must be of the same
dimensions as those required for initializers in astra_mex_data3d('create').
Additionally, it must be of class 'single'.

The optional argument 'readonly' (default: false), controls the exact
behaviour of this operation. See the two sections below for details.

The optional argument 'Z' (default: 0) allows creating a data object that is smaller
in the third dimension than the Matlab array. The data object will be mapped
to slices starting at slice Z. NB: Z is zero-based, unlike matlab array indexing.

**Read-only link mode:**

The data object becomes an additional reference to the
array, effectively behaving the same as a Matlab assignment
'internal_data = A;' (if 'A' is passed as the 'array' argument). If the array
A is changed inside Matlab, a copy will be made and the changes to A will not
be visible to this data object.

The data object's read-only state is not enforced by the astra toolbox. Using
it as output for algorithms is allowed, but the exact effects depend on
Matlab's internal reference counting mechanics.

**Read-write link mode:**

The passed array is 'unshared' and the data object
obtains a second reference to this array. There is no direct Matlab
script equivalent to this, but effectively the data object and the passed array
will share memory. Any changes to the data object from inside the toolbox will
be visible in Matlab.

If the passed array is modified in Matlab, this link is broken (by matlab's
reference counting mechanism), and the changes will not be visible to
the astra data object.

**python link**

.. code-block:: python

  id = astra.data3d.link('-vol', vol_geom, data)
  id = astra.data3d.link('-proj3d', proj_geom, data)

This creates an Astra data object that shares its memory with the numpy.ndarray data.
The ndarray must be contiguous and of dtype float32, and of the right shape.

Changes to the ndarray will be visible to Astra, and vice versa.

This increments the reference count of the underlying memory, so it is safe to
delete the data ndarray while the Astra object still exists.

**python get_shared**

.. code-block:: python

  A = astra.data3d.get_shared(id)

This fetches the data object as a numpy.ndarray that shares its memory with the Astra object.

Changes to the returned ndarray will be visible to Astra, and vice versa.

Deleting the Astra object while the resulting Python object still exists will
lead to undefined behaviour and potentially memory corruption and crashes.
