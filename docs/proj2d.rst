2D Projectors
=============

Projectors define the parameters of the forward and backward projection operations,
such as how projection weights are calculated, whether CUDA or CPU execution is used,
and other parameters.

All CPU reconstruction algorithms require a projector, and each projector is only
suitable for either parallel or fanflat projection geometries. On the other hand,
CUDA algorithms always use `"cuda" <#cuda-projector>`_ projector, which corresponds to
accelerated `"linear" <#linear-projector>`_ (Joseph/slice-interpolated) projector type.

Creation
--------

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id = astra.create_projector(projector_type, proj_geom, vol_geom)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      id = astra_create_projector(projector_type, proj_geom, vol_geom);

This function is a wrapper around :ref:`create <create-projector-2d>` method with a more
convenient interface.

The allocated object can be freed after use with a call to:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.projector.delete(id)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_projector('delete', id)



Parallel beam
-------------

The projectors in this section can be used with the parallel projection geometry.

line
~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_id = astra.create_projector('line', proj_geom, vol_geom)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      proj_id = astra_create_projector('line', proj_geom, vol_geom);

The weight of a ray/pixel pair is given by the length of the
intersection of the pixel and the ray, considered as a zero-thickness line.


strip
~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_id = astra.create_projector('strip', proj_geom, vol_geom)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      proj_id = astra_create_projector('strip', proj_geom, vol_geom);

The weight of a ray/pixel pair is given by the area of the
intersection of the pixel and the ray, considered as a strip with the same
width as a detector pixel.


.. _linear-projector:

linear
~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_id = astra.create_projector('linear', proj_geom, vol_geom)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      proj_id = astra_create_projector('linear', proj_geom, vol_geom);

A ray is traced through successive columns or rows (depending on which are
most orthogonal to the ray). The contribution of this column/row to this ray
is then given by linearly interpolating between the two nearest volume
pixels of the intersection of the ray and the column/row.

This is also known as the Joseph kernel, or a slice-interpolated kernel.


Fan beam
--------

line_fanflat
~~~~~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_id = astra.create_projector('line_fanflat', proj_geom, vol_geom)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      proj_id = astra_create_projector('line_fanflat', proj_geom, vol_geom);

The weight of a ray/pixel pair is given by the length of the
intersection of the pixel and the ray, considered as a zero-thickness line.
This projector can be used with the fanflat and fanflat_vec geometries.


strip_fanflat
~~~~~~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_id = astra.create_projector('strip_fanflat', proj_geom, vol_geom)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      proj_id = astra_create_projector('strip_fanflat', proj_geom, vol_geom);

The weight of a ray/pixel pair is given by the area of the
intersection of the pixel and the ray. The ray is considered as a 2D cone
from the source to the full width of the detector pixel. The projector can only
be used with the fanflat geometry.

.. note::
  This mathematical model does not properly take into account the fan beam
  magnification effect.


Miscellaneous
-------------

sparse_matrix
~~~~~~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_id = astra.create_projector('sparse_matrix', proj_geom, vol_geom)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      proj_id = astra_create_projector('sparse_matrix', proj_geom, vol_geom);

This projector uses a sparse matrix projection geometry. See the
documentation for that geometry for details.

.. _cuda-projector:

cuda
~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_id = astra.create_projector('cuda', proj_geom, vol_geom)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      proj_id = astra_create_projector('cuda', proj_geom, vol_geom);

This projector corresponds to the `linear <#linear-projector>`_ (Joseph/slice-interpolated)
projector type accelerated using GPU/CUDA. It can be used with parallel, parallel_vec,
fanflat and fanflat_vec projection geometries.

The "cuda" projector type supports additional options that can be specified in the
``options`` field in the config passed to the :ref:`create <create-projector>` method,
or as ``options`` argument to the `create_projector <#creation>`_ function:

.. list-table::
   :header-rows: 1

   * - Name
     - Description

   * - *VoxelSuperSampling*
     - Each voxel in the volume will be subdivided by this factor along each
       dimension. This should only be used if voxels in the volume are
       larger than the detector pixels (default: 1).

   * - *DetectorSuperSampling*
     - Each detector pixel in the projection will be subdivided by this factor along
       each dimension. This should only be used if detector pixels are larger than the
       voxels in the volume (default: 1).

   * - *GPUIndex*
     - The index of the GPU to use (default: 0).

.. note::
  Note that these options have lower priority than the options you can define when
  configuring the `algorithms <algs/index.html>`_.


API
---

.. _create-projector-2d:

create
~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

       config = {
           "type": "line",
           "ProjectionGeometry": proj_geom,
           "VolumeGeometry": vol_geom
       }
       id = astra.projector.create(config)

  .. group-tab:: MATLAB
    .. code-block:: matlab

       config.type = 'line';
       config.ProjectionGeometry = proj_geom;
       config.VolumeGeometry = vol_geom;
       id = astra_mex_projector('create', config);

Create a projector from a config object. This is called internally by
`create_projector <#creation>`_, which is the recommended way to create most projectors.


direct_FP / direct_BP
~~~~~~~~~~~~~~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

       astra.projector.direct_FP(projector_id, volume_array, out=projection_array)
       astra.projector.direct_BP(projector_id, projection_array, out=volume_array)

  .. group-tab:: MATLAB

       Not available in MATLAB.

Perform a forward/backprojection of the given array and write the result to another
array. Unlike the `forward projection algorithm <algs/FP_CUDA.html>`_, this function
does not require separate data and algorithm objects to be created within ASTRA.
Instead, it accepts any CPU/CUDA arrays supporting `DLPack standard
<https://github.com/dmlc/dlpack>`_ (NumPy, PyTorch, JAX, etc.)

.. note::
  Output projection/volume array can only be passed as an ``out`` keyword argument to
  avoid ambiguity.

.. note::
  Only "cuda" projector type is supported with CUDA arrays, but CPU arrays can be used
  with any projector type including "cuda".


matrix
~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      matrix_id = astra.projector.matrix(projector_id)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      matrix_id = astra_mex_projector('matrix', projector_id);

Create an explicit sparse matrix for the weight matrix encoded by this
projector.

This is only implemented for 2D CPU projectors.

The returned matrix_id can be further manipulated with
`astra.matrix/astra_mex_matrix <misc.html#projection-matrix-objects>`_. In
particular, it can be retrieved as a Python scipy or MATLAB sparse matrix with

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      S = astra.matrix.get(matrix_id)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      S = astra_mex_matrix('get', matrix_id);

It has to be freed after use with

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.matrix.delete(matrix_id)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_matrix('delete', matrix_id);

.. warning::
  Such a matrix can require significant memory for large geometries.


volume_geometry
~~~~~~~~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      vol_geom = astra.projector.volume_geometry(id)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      vol_geom = astra_mex_projector('volume_geometry', id);

Get the volume geometry attached to the given projector object.

.. note::
  The returned geometry may slightly deviate from the original geometry because of
  internal data type conversions.


projection_geometry
~~~~~~~~~~~~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_geom = astra.projector.projection_geometry(id)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      proj_geom = astra_mex_projector('projection_geometry', id);

Get the projection geometry attached to the given projector object.

.. note::
  The returned geometry may slightly deviate from the original geometry because of
  internal data type conversions.


is_cuda
~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      c = astra.projector.is_cuda(id)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      c = astra_mex_projector('is_cuda', id);

Return if the the projector is a CUDA projector.


delete
~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.projector.delete(id)
      astra.projector.delete([id1, id2, ...])

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_projector('delete', id)
      astra_mex_projector('delete', id1, id2, ...)

Free a single or multiple projector(s).


clear
~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

     astra.projector.clear()

  .. group-tab:: MATLAB
    .. code-block:: matlab

     astra_mex_projector('clear')

Free all projectors.


info
~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.projector.info()

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_projector('info')

Print basic information about all allocated projector objects.
