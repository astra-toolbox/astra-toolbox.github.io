Miscellaneous
=============

Testing the ASTRA installation
------------------------------

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.test()

  .. group-tab:: Matlab
    .. code-block:: matlab

      astra_test

This will run quick tests of basic CPU/GPU functionality, and report on
the results. As a part of this it will report if GPU functionality is
available.

Setting GPU index
-----------------

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.set_gpu_index(index)
      astra.set_gpu_index([index1, index2, ...])

  .. group-tab:: Matlab
    .. code-block:: matlab

      astra_mex('set_gpu_index', index);
      astra_mex('set_gpu_index', [index1 index2 ...]);

This lets ASTRA use the GPU with the specified index or indices. Not all ASTRA functionality supports
using multiple GPUs. In that case the GPU specified first will be used.


Masks
-----

Various reconstruction algorithms support projection data and reconstruction
volume masks. These behave as follows.

The projection data elements corresponding to locations with SinogramMask
value 0.0 will be ignored during the reconstruction. Similarly,
the reconstruction data elements corresponding to locations with
ReconstructionMask value 0.0 will be ignored during the reconstruction, and
their values will be preserved. (Mostly, see note on constraints below.)

The algorithm will behave as if the rows and columns corresponding to the
masked voxels and projection data elements have been removed from the
projection matrix entirely. In other words, it will iteratively try
to match the projection of the non-masked voxels to the non-masked projection
data elements.

NB: MinConstraint/MaxConstraint will affect even masked voxels.

astra_struct
------------

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      cfg = astra.astra_dict('NAME')

  .. group-tab:: Matlab
    .. code-block:: matlab

      cfg = astra_struct('NAME');

This is the basic script to create a configuration struct for many astra objects.
The returned struct is usually filled with more options after creating it, and then
passed to astra functions such as

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id = astra.algorithm.create(cfg)
      id = astra.projector.create(cfg)

  .. group-tab:: Matlab
    .. code-block:: matlab

      id = astra_mex_algorithm('create', cfg);
      id = astra_mex_projector('create', cfg);

The most common usage is for creating algorithm configuration structs. See the pages
on [2D CPU Algorithms], [2D GPU Algorithms], [3D GPU Algorithms] for available
algorithms, and the pages for the individual algorithms for the options they support.

astra_mex_matrix
----------------

astra_mex_matrix is used to manage sparse matrices. These can be created by the ASTRA toolbox itself to obtain
explicit weight matrices (see [astra_mex_projector]), or you can create them yourself for use with the sparse_matrix projection geometry.

It is a wrapper around the MEX file astra_mex_matrix_c.

astra_mex_matrix contains the following commands:

*    create
*    get
*    get_size
*    store
*    delete
*    clear
*    info

**create**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id = astra.matrix.create(S)

  .. group-tab:: Matlab
    .. code-block:: matlab

      id = astra_mex_matrix('create', S);

Create an ASTRA sparse matrix object from a Python sparse matrix of type scipy.sparse.csr_matrix or a Matlab sparse matrix.

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      S = astra.matrix.get(id)

  .. group-tab:: Matlab
    .. code-block:: matlab

      S = astra_mex_matrix('get', id);

Return an ASTRA sparse matrix object as a Python sparse matrix of type scipy.sparse.csr_matrix or a Matlab sparse matrix.

**get_size**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      s = astra.matrix.get_size(id)

  .. group-tab:: Matlab
    .. code-block:: matlab

      s = astra_mex_matrix('get_size', id);

Get the size (rows,columns) of the sparse matrix object.

**store**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.matrix.store(id, S)

  .. group-tab:: Matlab
    .. code-block:: matlab

      astra_mex_matrix('store', id, S);

Store a new Python or Matlab sparse matrix in an ASTRA sparse matrix object.

NB: This does not re-allocate memory: the number of rows and
non-zero entries may not be larger than they were when
the object was first created.

**delete**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.matrix.delete(id)
      astra.matrix.delete([id1, id2, ...])

  .. group-tab:: Matlab
    .. code-block:: matlab

      astra_mex_matrix('delete', id)

Free a single sparse matrix.

**clear**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.matrix.clear()

  .. group-tab:: Matlab
    .. code-block:: matlab

      astra_mex_matrix('clear')

Free all sparse matrices.

**info**

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.matrix.info()

  .. group-tab:: Matlab
    .. code-block:: matlab

      astra_mex_matrix('info')

Print basic information about all allocated sparse matrix objects.

