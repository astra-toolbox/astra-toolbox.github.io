2D Projectors
=============

Projectors determine the (implicit) weight matrix of the geometries.
All CPU reconstruction algorithms require a projector.

Each projector is only suitable for specific projection geometries.

Creation
--------

.. code-block:: matlab

 id = astra_create_projector(...);

Create a projector object. Projectors determine the (implicit) weight matrix of
the geometries. All CPU reconstruction algorithms require a projector.

This script is a wrapper around astra_mex_projector('create', ...) with a more
convenient interface. See below for specifics.

The allocated object must be freed after use with a call to

.. code-block:: matlab

 astra_mex_projector('delete', id)



Parallel beam
-------------

The projectors in this section can be used with the parallel projection geometry.

**line**

.. code-block:: matlab

 proj_id = astra_create_projector('line', proj_geom, vol_geom);

The weight of a ray/pixel pair is given by the length of the
intersection of the pixel and the ray, considered as a zero-thickness line.

**strip**

.. code-block:: matlab

 proj_id = astra_create_projector('strip', proj_geom, vol_geom);

The weight of a ray/pixel pair is given by the area of the
intersection of the pixel and the ray, considered as a strip with the same
width as a detector pixel.

**linear**

.. code-block:: matlab

 proj_id = astra_create_projector('linear', proj_geom, vol_geom);

A ray is traced through successive columns or rows (depending on which are
most orthogonal to the ray). The contribution of this column/row to this ray
is then given by linearly interpolating between the two nearest volume
pixels of the intersection of the ray and the column/row.

This is also known as the Joseph kernel, or a slice-interpolated kernel.

Fan beam
--------

**line_fanflat**

.. code-block:: matlab

 proj_id = astra_create_projector('line_fanflat', proj_geom, vol_geom);

The weight of a ray/pixel pair is given by the length of the
intersection of the pixel and the ray, considered as a zero-thickness line.
This projector can be used with the fanflat and fanflat_vec geometries.

**strip_fanflat**

.. code-block:: matlab

 proj_id = astra_create_projector('strip_fanflat', proj_geom, vol_geom);

The weight of a ray/pixel pair is given by the area of the
intersection of the pixel and the ray. The ray is considered as a 2D cone
from the source to the full width of the detector pixel. The projector can only
be used with the fanflat geometry.

NB: This mathematical model does not properly take into account the fan beam
magnification effect.

Miscellaneous
-------------

**sparse_matrix**

.. code-block:: matlab

 proj_id = astra_create_projector('sparse_matrix', proj_geom, vol_geom);

This projector uses a sparse matrix projection geometry. See the
documentation for that geometry for details.

**cuda**

.. code-block:: matlab

 proj_id = astra_create_projector('cuda', proj_geom, vol_geom);

This projector does not directly specify a weight matrix, but instead
is intended to let algorithms use GPU/CUDA code. It can be used
with parallel, fanflat and fanflat_vec projection geometries.

NB: This functionality has not yet been implemented everywhere. This projector
is currently only usable in astra_create_sino and astra_create_backprojection.

astra_mex_projector
-------------------

astra_mex_projector is used to manage projector objects.

It is a wrapper around the MEX file astra_mex_projector_c.

astra_mex_projector contains the following commands.

*    create
*    matrix
*    volume_geometry
*    projection_geometry
*    is_cuda
*    delete
*    clear
*    info

**create**

.. code-block:: matlab

 id = astra_mex_projector('create', cfg);

Create a projector from a config object. This is called internally
by the script [[astra_create_projector]], which is the recommended way
to create most projectors. 

**matrix**

.. code-block:: matlab

 matrix_id = astra_mex_projector('matrix', projector_id);

Create an explicit sparse matrix for the weight matrix encoded by this
projector.

This is only implemented for 2D CPU projectors.

The returned matrix_id can be further manipulated with [astra_mex_matrix].
In particular, it can be retrieved as a Matlab sparse matrix with

.. code-block:: matlab

 S = astra_mex_matrix('get', matrix_id);

It has to be freed after use with

.. code-block:: matlab

 astra_mex_matrix('delete', matrix_id);

NB: Such a matrix can be very large for large geometries.

**volume_geometry**

.. code-block:: matlab

 vol_geom = astra_mex_projector('volume_geometry', id);

Get the volume geometry attached to the given projector object.

NB: This is not fully implemented yet and the return value may not accurately represent the geometry.

**projection_geometry**

.. code-block:: matlab

 proj_geom = astra_mex_projector('projection_geometry', id);

Get the projection geometry attached to the given projector object.

NB: This is not fully implemented yet and the return value may not accurately represent the geometry.

**is_cuda**

.. code-block:: matlab

 c = astra_mex_projector('is_cuda', id);

Return if the the projector is a CUDA projector.

**delete**

.. code-block:: matlab

 astra_mex_projector('delete', id)
 astra_mex_projector('delete', id1, id2, ...)

Free a single or multiple projector(s).

**clear**

.. code-block:: matlab

 astra_mex_projector('clear')

Free all projectors.

**info**

.. code-block:: matlab

 astra_mex_projector('info')

Print basic information about all allocated projector objects.
