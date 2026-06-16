Miscellaneous
=============

Testing the ASTRA installation
------------------------------

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.test()

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_test

This will run quick tests of basic CPU/GPU functionality, and report on
the results. As a part of this it will report if GPU functionality is
available.


Working with large data
-----------------------

In case the data size is so large that it doesn't fit into the GPU memory, ASTRA will
automatically split and process the data in subsets. This functionality is only
available for FP3D, BP3D and FDK algorithms.

.. warning::

  At the moment, if the input/output data is a `linked <data3d.html#link>`_
  GPU tensor, the automatic splitting will not work correctly.

.. tip::

  Other GPU libraries, such as PyTorch, often allocate more GPU memory then
  they actually need to speed up computations. This significantly reduces the memory
  available to ASTRA, and usually it's a good idea to shrink the memory pool on the
  external library side (e.g. with ``torch.cuda.empty_cache``) before calling ASTRA if you
  get out-of-memory errors.


.. _set-single-gpu:

Choosing the GPU to use
-----------------------

On systems equipped with several GPUs, you can specify which GPU will be used by ASTRA
with:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.set_gpu_index(index)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex('set_gpu_index', index);

.. _set-multi-gpu:

Using several GPUs cooperatively
--------------------------------

ASTRA can utilize several GPUs simultaneously for a single algorithm to speed up the
computation. To do that, you can define the desired set of GPUs to be used:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.set_gpu_index([index1, index2, ...])

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex('set_gpu_index', [index1 index2 ...]);

.. warning::

  At the moment, only FP3D, BP3D and FDK algorithms support this
  functionality. For the rest, the first GPU in the specified index list will be used as
  the fallback.

.. warning::

  In `multithreading`_ contexts (Python), the GPU indices will be
  set globally (only) when specifying multiple indices, so it cannot be used to
  reliably restrict a thread to a given set of GPUs. If this is required, you can
  pass the set of GPUs as an option when creating the Astra projector or
  algorithm object.



Multithreading
--------------

In Python, Astra supports concurrent execution of its algorithms in multiple threads. This
is enabled by Astra dropping the Python global interpreter lock while executing an Astra algorithm.
This functionality is useful to process different inputs on different GPUs simultaneously.
Another use case is processing data blocks on the *same* GPU in parallel, which is useful
when a single ASTRA call under-utilizes the GPU. For instance, here is how one can
compute a fan-parallel projection using multithreading:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      import astra
      import numpy as np
      from concurrent.futures import ThreadPoolExecutor

      N = 512
      vol_geom_full = astra.create_vol_geom(N, N, N)
      vol_geom_slice = astra.create_vol_geom(N, N)
      angles = np.linspace(0, 2*np.pi, N, endpoint=False)
      proj_geom_slice = astra.create_proj_geom('fanflat', 1.0, N, angles, N, N)
      projector = astra.create_projector('cuda', proj_geom_slice, vol_geom_slice)

      phantom_id, phantom_data = astra.data3d.shepp_logan(vol_geom_full)

      def forward_project_slice(vol_slice):
          slice_proj_id, slice_proj_data = astra.create_sino(vol_slice, projector)
          astra.data2d.delete(slice_proj_id)
          return slice_proj_data

      with ThreadPoolExecutor(max_workers=16) as executor:
          projections = list(executor.map(forward_project_slice, phantom_data))
      projections = np.stack(projections, axis=0)

  .. group-tab:: MATLAB

      MATLAB doesn't support executing external libraries in multithreaded environments.

.. warning::

  No special care is taken about race conditions, so the user has to
  ensure that the outputs of algorithms are not accessed before they are
  finished, objects are created before they are used, and objects are not deleted
  while still in use by other threads.

.. warning::

  MATLAB doesn't support executing external libraries in multithreaded
  environments, so the only option is much less lightweight process-based concurrent
  execution. The simplest approach is to just start several copies of MATLAB in batch
  mode.

Python free threading
---------------------

Since Python 3.13, there is a separate Python interpreter that disables the global interpreter lock, called
`free threading <https://docs.python.org/3/howto/free-threading-python.html>`_.
Astra supports this mode, and most Astra functions are thread-safe.

There are two exceptions to the thread-safety of Astra calls: setting logging
parameters via the astra.log module is not thread-safe, and neither is
:ref:`passing multiple GPU indices to astra.set_gpu_index <set-multi-gpu>`.

On the other hand, using :ref:`astra.set_gpu_index with a single GPU index <set-single-gpu>` is
thread-safe, and will set the active GPU only for the current thread.

.. warning::

  No special care is taken about race conditions, so the user has to
  ensure that the outputs of algorithms are not accessed before they are
  finished, objects are created before they are used, and objects are not deleted
  while still in use by other threads.


Masks
-----

Various reconstruction algorithms support binary projection data and
reconstruction volume masks. These behave as follows.

The projection data and reconstruction masks are specified in SinogramMask and
ReconstructionMask, respectively. Each element of these masks must be either
0.0 or 1.0.

The projection data elements corresponding to locations with SinogramMask
value 0.0 will be ignored during the reconstruction. Similarly,
the reconstruction data elements corresponding to locations with
ReconstructionMask value 0.0 will be ignored during the reconstruction, and
their values will be preserved (mostly, see notes below).

The algorithm will behave as if the rows and columns corresponding to the
masked voxels and projection data elements have been removed from the
projection matrix entirely. In other words, it will iteratively try
to match the projection of the non-masked voxels to the non-masked projection
data elements.

.. warning::

  MinConstraint/MaxConstraint will affect even masked voxels.

.. warning::

  FP and BP algorithms (CPU versions) overwrite the output, so the values
  outside the sinogram/reconstruction masks, respectively, will be set to zero
  instead of being ignored.

ASTRA configuration structure
-----------------------------

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      cfg = astra.astra_dict('NAME')

  .. group-tab:: MATLAB
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

  .. group-tab:: MATLAB
    .. code-block:: matlab

      id = astra_mex_algorithm('create', cfg);
      id = astra_mex_projector('create', cfg);

The most common usage is for creating algorithm configuration structs. See the
pages for `individual algorithms <algs/index.html>`_ for the options they
support.


Projection matrix objects
-------------------------

Matrix objects can be created by the ASTRA toolbox to obtain explicit weight
matrices (see `here <proj2d.html#api>`_), or you can define them yourself for
use with the ``sparse_matrix`` projection geometry. Matrix objects can be
manipulated using the following commands:

create
~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id = astra.matrix.create(S)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      id = astra_mex_matrix('create', S);

get
~~~

Create an ASTRA sparse matrix object from a Python sparse matrix of type scipy.sparse.csr_matrix or a MATLAB sparse matrix.

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      S = astra.matrix.get(id)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      S = astra_mex_matrix('get', id);

Return an ASTRA sparse matrix object as a Python sparse matrix of type scipy.sparse.csr_matrix or a MATLAB sparse matrix.


get_size
~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      s = astra.matrix.get_size(id)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      s = astra_mex_matrix('get_size', id);

Get the size (rows,columns) of the sparse matrix object.


store
~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.matrix.store(id, S)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_matrix('store', id, S);

Store a new Python or MATLAB sparse matrix in an ASTRA sparse matrix object.

.. warning::
  This does not re-allocate memory: the number of rows and non-zero entries may not be
  larger than they were when the object was first created.


delete
~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.matrix.delete(id)
      astra.matrix.delete([id1, id2, ...])

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_matrix('delete', id)

Free a single sparse matrix.


clear
~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.matrix.clear()

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_matrix('clear')

Free all sparse matrices.


info
~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.matrix.info()

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_matrix('info')

Print basic information about all allocated sparse matrix objects.
