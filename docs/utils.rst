ASTRA utility scripts
=====================

Create sinogram
---------------

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id, sinogram = astra.create_sino(data, projector_id)
      id = astra.create_sino(data, projector_id, returnData=False)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      id = astra_create_sino(data, projector_id);
      [id, sinogram] = astra_create_sino(data, projector_id);

Compute a sinogram from a 2D volume using the given projector.
See the `documentation <proj2d.html>`_` for details on projectors.
The projector_id may be for either a CPU or a GPU/CUDA projector.

The input may either be an `astra.data2d/astra_mex_data2d <data2d.html>`_ ID for
a volume data object of the right volume geometry, or a matrix directly
containing the object data. In the latter case, it must be of size
(height, width) as defined in the volume geometry. It must be of class single,
double or logical.

In its one output argument form, returns a newly allocated
`astra.data2d/astra_mex_data2d <data2d.html>`_ ID of the object containing the
sinogram.

In its two output argument form, additionally returns the sinogram data itself.

Create backprojection
---------------------

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id, volume = astra.create_backprojection(data, projector_id)
      id = astra.create_backprojection(data, projector_id, returnData=False)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      id = astra_create_backprojection(data, projector_id);
      [id, volume] = astra_create_backprojection(data, projector_id);

Compute a backprojection of a 2D sinogram using the given projector.
See the `documentation <proj2d.html>`_` for details on projectors.
The projector_id may be for either a CPU or a GPU/CUDA projector.

The input may either be an `astra.data2d/astra_mex_data2d <data2d.html>`_ ID for
a projection data object of the right projection geometry, or a matrix directly
containing the sinogram. In the latter case, it must be of size (#angles,
#detector pixels) as defined in the projection geometry. It must be of class
single, double or logical.

In its one output argument form, returns a newly allocated
`astra.data2d/astra_mex_data2d <data2d.html>`_ ID of the object containing the
volume.

In its two output argument form, additionally returns the volume data itself.

Create sinogram (CUDA)
----------------------

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      N/A

  .. group-tab:: MATLAB
    .. code-block:: matlab

      id = astra_create_sino_cuda(data, proj_geom, vol_geom);
      [id, sinogram] = astra_create_sino_cuda(data, proj_geom, vol_geom);

Compute a sinogram from a 2D volume and the given geometry, using a GPU.

The input may either be an `astra_mex_data2d <data2d.html>`_ ID for a volume
data object of the right volume geometry, or a matrix directly containing the
object data. In the latter case, it must be of size (height, width) as defined
in the volume geometry. It must be of class single, double or logical.

In its one output argument form, returns a newly allocated `astra_mex_data2d
<data2d.html>`_ ID of the object containing the sinogram.

In its two output argument form, additionally returns the sinogram data itself.

Create backprojection (CUDA)
----------------------------

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      N/A

  .. group-tab:: MATLAB
    .. code-block:: matlab

      volume = astra_create_backprojection_cuda(data, proj_geom, vol_geom);

Compute a backprojection of a 2D sinogram and the given geometry, using a GPU.

The input may either be an `astra_mex_data2d <data2d.html>`_ ID for a projection
data object of the right projection geometry, or a matrix directly containing
the sinogram. In the latter case, it must be of size (#angles, #detector pixels)
as defined in the projection geometry. It must be of class single, double or
logical.

Returns volume data.

Note: as a historical accident, this function has a different return
argument signature than the other astra_create_backprojection* functions.

Create 3D sinogram (CUDA)
-------------------------

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id, volume = astra.create_sino3d_gpu(data, proj_geom, vol_geom)
      id = astra.create_sino3d_gpu(data, proj_geom, vol_geom, returnData=False)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      id = astra_create_sino3d_cuda(data, proj_geom, vol_geom);
      [id, projdata] = astra_create_sino3d_cuda(data, proj_geom, vol_geom);

Compute projection data from a 3D volume and the given geometry, using a GPU.

The input may either be an `astra.data3d/astra_mex_data3d <data3d.html>`_ ID for
a volume data object of the right volume geometry, or a matrix directly
containing the object data. In the latter case, it must be of size (x, y, z) as
defined in the volume geometry. It must be of class single, double or logical.

In its one output argument form, returns a newly allocated
`astra.data3d/astra_mex_data3d <data3d.html>`_ ID containing the projection
data.

In its two output argument form, additionally returns the projection data
itself.

Create 3D backprojection (CUDA)
-------------------------------

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      id, volume = astra.create_backprojection3d_gpu(data, proj_geom, vol_geom)
      id = astra.create_backprojection3d_gpu(data, proj_geom, vol_geom, returnData=False)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      id = astra_create_backprojection3d_cuda(data, proj_geom, vol_geom);
      [id, volume] = astra_create_backprojection(data, proj_geom, vol_geom);

Compute a backprojection of 3D projection data and the given geometry, using
a GPU.

The input may either be an `astra.data3d/astra_mex_data3d <data3d.html>`_ ID for
a projection data object of the right projection geometry, or a matrix directly
containing the projection data. In the latter case, it must be of size (u,
#angles, v), where u is the number of columns of the detector and v the number
of rows as defined in the projection geometry. It must be of class single,
double or logical.

In its one output argument form, returns a newly allocated
`astra.data3d/astra_mex_data3d <data3d.html>`_ ID of the object containing the
volume.

In its two output argument form, additionally returns the volume data itself.

Convert geometry to vector representation
-----------------------------------------

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_geom_vec = astra.geom_2vec(proj_geom)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      proj_geom_vec = astra_geom_2vec(proj_geom);

Convert a projection geometry of type fanflat, cone, or parallel3d into
an equivalent geometry of type fanflat_vec, cone_vec, or parallel3d_vec,
respectively.

Geometry post-alignment
-----------------------

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      proj_geom = astra.geom_postalignment(proj_geom, factorU)
      proj_geom = astra.geom_postalignment(proj_geom, [factorU, factorV])

  .. group-tab:: MATLAB
    .. code-block:: matlab

      proj_geom = astra_geom_postalignment(proj_geom, factorU)
      proj_geom = astra_geom_postalignment(proj_geom, [factorU factorV])

Apply a postalignment to a projection geometry. Can be used to model the
rotation axis offset.

For 2D geometries, the argument factor is a single float specifying the
distance to shift the detector (measured in detector pixels).
For 3D geometries, factor is a pair of floats specifying the horizontal
resp. vertical distances to shift the detector. If only a single float is
specified, this is treated as an horizontal shift.

Get geometry size
-----------------

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

      s = astra.geom_size(geom)
      s = astra.geom_size(geom, dim)

  .. group-tab:: MATLAB
    .. code-block:: matlab

      s = astra_geom_size(geom);
      s = astra_geom_size(geom, dim);

Get the size of MATLAB arrays for data objects with a given geometry.
All geometries (2D, 3D, volume, projection) are supported.

The size returned is the size needed for arrays passed to the 2D/3D data
'create', 'set'/'store' and 'link' commands, as well as the size of arrays
returned by 'get'/'get_single' commands.
