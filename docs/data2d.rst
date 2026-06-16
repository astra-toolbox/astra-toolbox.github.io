2D Data Objects
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

      id = astra.data2d.create('-vol', vol_geom)
      id = astra.data2d.create('-vol', vol_geom, initializer)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      id = astra_mex_data2d('create', '-vol', vol_geom);
      id = astra_mex_data2d('create', '-vol', vol_geom, initializer);

Create an initialized 2D volume data object for the geometry ``vol_geom``.

Initializer may be:

* a scalar: the object is initialized with this constant value.
* a matrix: the object is initialized with the contents of this matrix.

  The matrix must be of size ``(y, x)`` as defined by the volume geometry.
  In Python, it must be convertible to ``float32``. In MATLAB, it must be of class
  ``single``, ``double`` or ``logical``.

If no initializer is given, the volume is initialized to all zeros.

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id = astra.data2d.create('-sino', proj_geom)
      id = astra.data2d.create('-sino', proj_geom, initializer)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      id = astra_mex_data2d('create', '-sino', proj_geom);
      id = astra_mex_data2d('create', '-sino', proj_geom, initializer);

Create an initialized 2D projection data object for the geometry ``proj_geom``.

Initializer may be:

* a scalar: the object is initialized with this constant value.
* a matrix: the object is initialized with the contents of this matrix.

  The matrix must be of size ``(angles, u)``, where ``u`` is the number of
  detector pixels defined by the projection geometry. In Python, it must be
  convertible to ``float32``. In MATLAB, it must be of class ``single``,
  ``double`` or ``logical``.

If no initializer is given, the projection data object is initialized to all zeros.


link
~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id = astra.data2d.link('-sino', proj_geom, array)
      id = astra.data2d.link('-vol', vol_geom, array)

  .. group-tab:: MATLAB

      Only available in Python interface.

Create an ASTRA data object that shares its memory with the specified CPU or GPU array
supporting the `DLPack interface <https://github.com/dmlc/dlpack>`_. This includes
arrays from NumPy, PyTorch, JAX, TensorFlow and CuPy libraries. The array must be
contiguous, have ``float32`` dtype, and a shape compatible with the specified geometry.
No copying is performed, so changes to the linked array will be visible to ASTRA, and
vice versa. Linking the array increments the reference count of the underlying memory,
so deleting the original array will not destroy the created ASTRA object.

.. versionadded:: 2.5
   Accept any 2D array supporting DLPack protocol. Before, only NumPy arrays were supported.


Phantom generation
------------------

ASTRA includes a built-in generator for the Shepp-Logan phantom:

shepp_logan
~~~~~~~~~~~

.. versionadded:: 2.2

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id, data = astra.data2d.shepp_logan(vol_geom, modified)

  .. group-tab:: MATLAB

      Only available in Python interface.

Create a Shepp-Logan phantom as an ASTRA volume data object (``id``), and a NumPy array
(``data``). ``modified=True`` creates a phantom with improved contrast (default).


API
---

get
~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      A = astra.data2d.get(id)

    Fetch the data object as a 2D array with ``float32`` dtype.

  .. group-tab:: MATLAB
    .. code-block:: matlab

      A = astra_mex_data2d('get', id);

    Fetch the data object as a 2D matrix of class ``double``.


get_shared
~~~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      A = astra.data2d.get_shared(id)

    Fetch the data object as a 2D NumPy array sharing its memory with the ASTRA object.
    Changes to the returned array will be visible to ASTRA, and vice versa. Deleting the
    ASTRA object while the resulting Python object still exists will lead to undefined
    behaviour and potentially memory corruption and crashes.

  .. group-tab:: MATLAB

    Only available in Python interface.


get_single
~~~~~~~~~~

.. tabs::
  .. group-tab:: Python

      Only available in MATLAB interface.

  .. group-tab:: MATLAB
    .. code-block:: matlab

      A = astra_mex_data2d('get_single', id);

    Fetch the data object as a 2D matrix of class ``single``.


set / store
~~~~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.data2d.store(id, A)


  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_data2d('set', id, A)
      astra_mex_data2d('store', id, A)

This stores the matrix ``A`` in the data object. The dimensions of ``A`` must match the
dimensions of the existing data object.

Set and store are synonyms in the MATLAB interface.


get_geometry
~~~~~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      geom = astra.data2d.get_geometry(id)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      geom = astra_mex_data2d('get_geometry', id);

Get the (volume or projection) geometry attached to the object.

.. note::
  The returned geometry may slightly deviate from the original geometry because of
  internal data type conversions.


change_geometry
~~~~~~~~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.data2d.change_geometry(id, geom)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_data2d('change_geometry', id, geom);

Change the (volume or projection) geometry attached to the object. It cannot change the
dimensions of the data object, but can be used to change pixel dimensions or projection
angles, for example.


delete
~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.data2d.delete(id)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_data2d('delete', id);

Free the memory of a data object.


clear
~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.data2d.clear()

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_data2d('clear');

Free all data objects.


info
~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.data2d.info()

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_data2d('info')

Print basic information about all allocated data objects.
