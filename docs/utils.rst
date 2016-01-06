ASTRA utility scripts
=====================

astra_create_sino
-----------------

.. code-block:: matlab

 id = astra_create_sino(data, projector_id);
 [id, sinogram] = astra_create_sino(data, projector_id);

Compute a sinogram from a 2D volume using the given projector.
See the documentation for astra_create_projector for details on projectors.
The projector_id may be for either a CPU or a GPU/CUDA projector.

The input may either be an astra_mex_data2d id for a volume data object
of the right volume geometry, or a matrix directly containing the object data.
In the latter case, it must be of size (height,width) as defined in the volume
geometry. It must be of class single, double or logical.

In its one output argument form, astra_create_sino returns a newly
allocated astra_mex_data2d id containing the sinogram.

In its two output argument form, astra_create_sino additionally returns
the sinogram itself.

astra_create_backprojection
---------------------------

.. code-block:: matlab

 id = astra_create_backprojection(data, projector_id);
 [id, volume] = astra_create_backprojection(data, projector_id);

Compute a backprojection of a 2D sinogram using the given projector.
See the documentation for astra_create_projector for details on projectors.
The projector_id may be for either a CPU or a GPU/CUDA projector.

The input may either be an astra_mex_data2d id for a projection data object
of the right projection geometry, or a matrix directly containing the sinogram.
In the latter case, it must be of size (#angles, #detector pixels) as defined
in the projection geometry. It must be of class single, double or logical.

In its one output argument form, astra_create_backprojection returns a newly
allocated astra_mex_data2d id containing the volume.

In its two output argument form, astra_create_backprojection additionally
returns the volume data itself.

astra_create_sino_cuda
----------------------

.. code-block:: matlab

 id = astra_create_sino(data, proj_geom, vol_geom);
 [id, sinogram] = astra_create_sino(data, proj_geom, vol_geom);

Compute a sinogram from a 2D volume and the given geometry, using a GPU.

The input may either be an astra_mex_data2d id for a volume data object
of the right volume geometry, or a matrix directly containing the object data.
In the latter case, it must be of size (height,width) as defined in the
volume geometry. It must be of class single, double or logical.

In its one output argument form, astra_create_sino_cuda returns a newly
allocated astra_mex_data2d id containing the sinogram.

In its two output argument form, astra_create_sino_cuda additionally returns
the sinogram itself.

astra_create_backprojection_cuda
--------------------------------

.. code-block:: matlab

 volume = astra_create_backprojection_cuda(data, proj_geom, vol_geom);

Compute a backprojection of a 2D sinogram and the given geometry, using a GPU.

The input may either be an astra_mex_data2d id for a projection data object
of the right projection geometry, or a matrix directly containing the sinogram.
In the latter case, it must be of size (#angles, #detector pixels) as
defined in the projection geometry. It must be of class single, double or logical.

astra_create_backprojection returns the volume data in a matrix.

Note: as a historical accident, this function has a different return
argument signature than the other astra_create_backprojection* functions.

astra_create_sino3d_cuda
------------------------

 id = astra_create_sino3d_cuda(data, proj_geom, vol_geom);
 [id, projdata] = astra_create_sino3d_cuda(data, proj_geom, vol_geom);

Compute projection data from a 3D volume and the given geometry, using a GPU.

The input may either be an astra_mex_data3d id for a volume data object
of the right volume geometry, or a matrix directly containing the object data.
In the latter case, it must be of size (x,y,z) as defined in the volume
geometry. It must be of class single, double or logical.

In its one output argument form, astra_create_sino3d_cuda returns a newly
allocated astra_mex_data3d id containing the projection data.

In its two output argument form, astra_create_sino3d_cuda additionally returns
the projection data itself.

astra_create_backprojection3d_cuda
----------------------------------

.. code-block:: matlab

 id = astra_create_backprojection3d_cuda(data, proj_geom, vol_geom);
 [id, volume] = astra_create_backprojection(data, proj_geom, vol_geom);

Compute a backprojection of 3D projection data and the given geometry, using
a GPU.

The input may either be an astra_mex_data3d id for a projection data object of
the right projection geometry, or a matrix directly containing the projection
data. In the latter case, it must be of size (u,#angles,v), where u is the
number of columns of the detector and v the number of rows as defined in the
projection geometry. It must be of class single, double or logical.

In its one output argument form, astra_create_backprojection3d_cuda returns a
newly allocated astra_mex_data3d id containing the volume.

In its two output argument form, astra_create_backprojection3d_cuda
additionally returns the volume data itself.

astra_geom_2vec
---------------

.. code-block:: matlab

 proj_geom_vec = astra_geom_2vec(proj_geom);

Convert a projection geometry of type fanflat, cone, or parallel3d into
an equivalent geometry of type fanflat_vec, cone_vec, or parallel3d_vec,
respectively.

astra_geom_size
---------------

.. code-block:: matlab

 s = astra_geom_size(geom);
 s = astra_geom_size(geom, dim);

Get the size of Matlab arrays for data objects with a given geometry.
All geometries (2D, 3D, volume, projection) are supported.

The size returned is the size needed for arrays passed to the
astra_mex_data2d/3d 'create', 'set'/'store' and 'link' functions, as well as the
size of arrays returned by astra_mex_data2d/3d 'get'/'get_single'.
