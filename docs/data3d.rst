3D Data Objects
===============

astra_mex_data3d
----------------

astra_mex_data3d is used to manage 3D data objects. It is a wrapper around the MEX file astra_mex_data3d_c.

3D data objects come in two varieties: volume data and projection data.

astra_mex_data3d has the following commands.

*    create
*    get
*    get_single
*    set / store
*    dimensions
*    delete
*    clear
*    info
*    link

**create**

.. code-block:: matlab

 id = astra_mex_data3d('create', '-vol', vol_geom);
 id = astra_mex_data3d('create', '-vol', vol_geom, initializer);

This creates an initialized 3D volume data object for the geometry vol_geom.

Initializer may be:

*    a scalar: the object is initialized with this constant value.
*    a matrix: the object is initialized with the contents of this matrix. The matrix must be of size (x,y,z) as defined in the volume geometry. It must be of class single, double or logical.

If an initializer is not present, the volume is initialized to zero.

.. code-block:: matlab

 id = astra_mex_data3d('create', '-proj3d', proj_geom);
 id = astra_mex_data3d('create', '-proj3d', proj_geom, initializer);

This creates an initialized 3D projection data object for the geometry proj_geom.

Initializer may be:

*    a scalar: the object is initialized with this constant value.
*    a matrix: the object is initialized with the contents of this matrix. The matrix must be of size (u,angles,v), where u is the number of columns of the detector and v the number of rows as defined in the projection geometry. It must be of class single, double or logical.

If an initializer is not present, the volume is initialized to zero.

**get**

.. code-block:: matlab

 A = astra_mex_data3d('get', id);

This fetches the data object as a 3D matrix of class double.

**get_single**

.. code-block:: matlab

 A = astra_mex_data3d('get_single', id);

This fetches the data object as a 3D matrix of class single.

**set / store**

.. code-block:: matlab

 astra_mex_data3d('set', id, A);
 astra_mex_data3d('store', id, A);

This stores the matrix A in the data object. The dimensions of A
must be the same as when used as an initializer in astra_mex_data3d('create').

Set and store are synonyms.

**dimensions**

.. code-block:: matlab

 s = astra_mex_data3d('dimensions', id);

Get the dimensions of a data object.

**delete**

.. code-block:: matlab

 astra_mex_data3d('delete', id);

Free the memory of a data object.

**clear**

.. code-block:: matlab

 astra_mex_data3d('clear');

Free all data objects.

**info**

.. code-block:: matlab

 astra_mex_data3d('info')

Print basic information about all allocated data objects.

**link**

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
