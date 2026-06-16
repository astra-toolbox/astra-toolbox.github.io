3D Projectors
=============

Projectors define the parameters of the forward and backward projection operations. In
3D algorithms, only "cuda3d" (accelerated linear/Joseph/slice-interpolated) projector
type is supported.

Creation
--------

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id = astra.create_projector("cuda3d", proj_geom, vol_geom)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      id = astra_create_projector("cuda3d", proj_geom, vol_geom);

This script is a wrapper around the :ref:`create <create-projector>` method with a more
convenient interface.

The allocated object can be freed after use with a call to:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.projector3d.delete(id)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_projector3d('delete', id)


Configuration options
---------------------

3D projectors support a ``ProjectionKernel`` option field in the config passed to the
:ref:`create <create-projector>` method. The options are:

- ``default`` : Standard forward/backprojection.
- ``2d_weighting`` : Use backprojection weighting optimized for 2D slices. This makes backprojection closer to the true adjoint of the forward projection when the number of slices is small, whereas the default weighting works better for large numbers of slices.
- ``matched_bp`` : In the backprojection, use the kernel producing the exact adjoint of the forward projection; slower and only available for 'cyl_cone_vec' geometry at the moment.


Additional options that can by specified in the ``options`` field in the config passed
to the :ref:`create <create-projector>` method, or as ``options`` argument to the
`create_projector <#creation>`_ function:

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

.. _create-projector:

create
~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

     cfg = {
         "type": "cuda3d",
         "ProjectionGeometry": proj_geom,
         "VolumeGeometry": vol_geom
     }
     id = astra.projector3d.create(cfg)

  .. group-tab:: MATLAB
    .. code-block:: matlab

     cfg.type = 'cuda3d';
     cfg.ProjectionGeometry = proj_geom;
     cfg.VolumeGeometry = vol_geom;
     id = astra_mex_projector3d('create', cfg);

Create a projector from a config object. This is called internally by
`create_projector <#creation>`_, which is the recommended way to create projectors.


direct_FP / direct_BP
~~~~~~~~~~~~~~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

       astra.projector3d.direct_FP(projector_id, volume_array, out=projection_array)
       astra.projector3d.direct_BP(projector_id, projection_array, out=volume_array)

  .. group-tab:: MATLAB

       Not available in MATLAB.

Perform a forward/backprojection of the given array and write the result to another
array. Unlike the `forward projection algorithm <algs/FP3D_CUDA.html>`_, this function
does not require separate data and algorithm objects to be created within ASTRA.
Instead, it accepts any CPU/CUDA arrays supporting `DLPack standard
<https://github.com/dmlc/dlpack>`_ (NumPy, PyTorch, JAX, etc.)

.. note::
  Output projection/volume array can only be passed as an ``out`` keyword argument to
  avoid ambiguity.


volume_geometry
~~~~~~~~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      vol_geom = astra.projector3d.volume_geometry(id)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      vol_geom = astra_mex_projector3d('volume_geometry', id);

Get the volume geometry attached to the given projector object.

.. note::
  The returned geometry may slightly deviate from the original geometry because of
  internal data type conversions.


projection_geometry
~~~~~~~~~~~~~~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_geom = astra.projector3d.projection_geometry(id)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      proj_geom = astra_mex_projector3d('projection_geometry', id);

Get the projection geometry attached to the given projector object.

.. note::
  The returned geometry may slightly deviate from the original geometry because of
  internal data type conversions.


delete
~~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.projector3d.delete(id)
      astra.projector3d.delete([id1, id2, ...])

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_projector3d('delete', id)
      astra_mex_projector3d('delete', id1, id2, ...)

Free a single or multiple projector(s).


clear
~~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

     astra.projector3d.clear()

  .. group-tab:: MATLAB
    .. code-block:: matlab

     astra_mex_projector3d('clear')

Free all projectors.


info
~~~~

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      astra.projector3d.info()

  .. group-tab:: MATLAB
    .. code-block:: matlab

      astra_mex_projector3d('info')

Print basic information about all allocated projector objects.
