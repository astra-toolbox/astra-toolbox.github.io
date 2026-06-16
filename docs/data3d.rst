3D Data Objects
===============

Volume and projection data
--------------------------

ASTRA data objects come in two varieties: volume data and projection data
(sinograms). A data object can be created using the following commands:

create
~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id = astra.data3d.create('-vol', vol_geom)
      id = astra.data3d.create('-vol', vol_geom, initializer)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      id = astra_mex_data3d('create', '-vol', vol_geom);
      id = astra_mex_data3d('create', '-vol', vol_geom, initializer);

Create an initialized 3D volume data object for the geometry ``vol_geom``.

Initializer may be:

*    a scalar: the object is initialized with this constant value.
*    a matrix: the object is initialized with the contents of this matrix.
     The matrix must be of size ``(n_slices, n_rows, n_cols)`` in Python, or
     ``(n_cols, n_rows, n_slices)`` in MATLAB, as defined by the volume
     geometry. In Python, it must be convertible to ``float32``. In MATLAB,
     it must be of class ``single``, ``double`` or ``logical``.

If no initializer is given, the volume is initialized to all zeros.

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id = astra.data3d.create('-proj3d', proj_geom)
      id = astra.data3d.create('-proj3d', proj_geom, initializer)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      id = astra_mex_data3d('create', '-proj3d', proj_geom);
      id = astra_mex_data3d('create', '-proj3d', proj_geom, initializer);

Create an initialized 3D projection data object for the geometry ``proj_geom``.

Initializer may be:

*    a scalar: the object is initialized with this constant value.
*    a matrix: the object is initialized with the contents of this matrix.
     The matrix must be of size ``(det_row_count, n_angles, det_col_count)``
     in Python, or ``(det_col_count, n_angles, det_row_count)`` in MATLAB,
     as defined by the projection geometry. In Python, it must be
     convertible to ``float32``. In MATLAB, it must be of class ``single``,
     ``double`` or ``logical``.

If no initializer is given, the projection data object is initialized to all zeros.


link
~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id = astra.data3d.link('-sino', proj_geom, array)
      id = astra.data3d.link('-vol', vol_geom, array)

  .. group-tab:: MATLAB

    :ref:`matlab_linking`

Create an ASTRA data object that shares its memory with the specified CPU or GPU array
supporting the `DLPack interface <https://github.com/dmlc/dlpack>`_. This includes
arrays from NumPy, PyTorch, JAX, TensorFlow and CuPy libraries. The array must be
contiguous, have ``float32`` dtype, and a shape compatible with the specified geometry.
No copying is performed, so changes to the linked array will be visible to ASTRA, and
vice versa. Linking the array increments the reference count of the underlying memory,
so deleting the original array will not destroy the created ASTRA object.

.. versionadded:: 2.2
   Accept any 3D array supporting DLPack protocol. Before, only NumPy arrays were supported.


Phantom generation
------------------

ASTRA includes a built-in generator for the 3D Shepp-Logan phantom:


shepp_logan
~~~~~~~~~~~

.. versionadded:: 2.2

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id, data = astra.data3d.shepp_logan(vol_geom, modified)

    Create a Shepp-Logan phantom as an ASTRA volume data object (``id``), and a
    NumPy array (``data``). ``modified=True`` creates a phantom with improved
    contrast (default).

  .. group-tab:: MATLAB

      Only available in Python interface.


API
---

get
~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      A = astra.data3d.get(id)

    Fetch the data object as a 3D array with dtype ``float32``.

  .. group-tab:: MATLAB
    .. code-block:: matlab

      A = astra_mex_data3d('get', id);

    Fetch the data object as a 3D matrix of class ``double``.


get_shared
~~~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      A = astra.data3d.get_shared(id)

    Fetch the data object as a 3D NumPy array sharing its memory with
    the ASTRA object. Changes to the returned array will be visible to ASTRA,
    and vice versa. Deleting the ASTRA object while the resulting Python
    object still exists will lead to undefined behaviour and potentially
    memory corruption and crashes.

  .. group-tab:: MATLAB

    Only available in Python interface.


get_single
~~~~~~~~~~

.. tabs::
  .. group-tab:: Python

    Only available in MATLAB interface.

  .. group-tab:: MATLAB
    .. code-block:: matlab

      A = astra_mex_data3d('get_single', id);

    Fetch the data object as a 3D matrix of class single.


set / store
~~~~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.data3d.store(id, A)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_data3d('set', id, A);
      astra_mex_data3d('store', id, A);

This stores the matrix ``A`` in the data object. The dimensions of ``A``
must match the dimensions of the existing data object.

Set and store are synonyms in the MATLAB interface.


dimensions
~~~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      s = astra.data3d.dimensions(id)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      s = astra_mex_data3d('dimensions', id);

Get the dimensions of a data object.


get_geometry
~~~~~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      geom = astra.data3d.get_geometry(id)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      geom = astra_mex_data3d('get_geometry', id);

Get the (volume or projection) geometry attached to the object.

.. note::
  The returned geometry may slightly deviate from the original geometry because of
  internal data type conversions.


change_geometry
~~~~~~~~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.data3d.change_geometry(id, geom)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_data3d('change_geometry', id, geom);

Change the (volume or projection) geometry attached to the object. It
cannot change the dimensions of the data object, but can be used to change
voxel dimensions or projection angles, for example.


delete
~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.data3d.delete(id)
      astra.data3d.delete([id1, id2, ...])

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_data3d('delete', id);

Free the memory of a data object.


clear
~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.data3d.clear()

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_data3d('clear');

Free all data objects.


info
~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.data3d.info()

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_data3d('info')

Print basic information about all allocated data objects.


.. _matlab_linking:

Linking data in MATLAB
----------------------

.. code-block:: matlab

 id = astra_mex_data3d('link', '-vol', vol_geom, array, readonly, Z);
 id = astra_mex_data3d('link', '-proj3d', proj_geom, array, readonly, Z);

This creates a data object that directly uses a MATLAB array as storage
instead of allocating its own memory. The array must be of the same
dimensions as those required for initializers in astra_mex_data3d('create').
Additionally, it must be of class 'single'.

The optional argument 'readonly' (default: false), controls the exact
behaviour of this operation. See the two sections below for details.

The optional argument 'Z' (default: 0) allows creating a data object that is smaller
in the third dimension than the MATLAB array. The data object will be mapped
to slices starting at slice Z. NB: Z is zero-based, unlike MATLAB array indexing.

**Read-only link mode:**

The data object becomes an additional reference to the
array, effectively behaving the same as a MATLAB assignment
'internal_data = A;' (if 'A' is passed as the 'array' argument). If the array
A is changed inside MATLAB, a copy will be made and the changes to A will not
be visible to this data object.

The data object's read-only state is not enforced by the astra toolbox. Using
it as output for algorithms is allowed, but the exact effects depend on
MATLAB's internal reference counting mechanics.

**Read-write link mode:**

.. warning::

  Functionality required for this link mode was removed from MATLAB since version 2020a.

The passed array is 'unshared' and the data object
obtains a second reference to this array. There is no direct MATLAB
script equivalent to this, but effectively the data object and the passed array
will share memory. Any changes to the data object from inside the toolbox will
be visible in MATLAB.

If the passed array is modified in MATLAB, this link is broken (by MATLAB's
reference counting mechanism), and the changes will not be visible to
the astra data object.
