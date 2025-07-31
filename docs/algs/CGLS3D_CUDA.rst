CGLS3D_CUDA
===========

This is a GPU implementation of the CGLS algorithm for 3D data sets.
It takes projection data and an initial reconstruction as input, and
returns the reconstruction after a specified number of CGLS iterations.

The internal state of the CGLS algorithm is reset every time
``astra.algorithm.run``/``astra_mex_algorithm('iterate')`` is called. This
implies that running CGLS for N iterations and then running it for another N
iterations may yield different results from running it 2N iterations at once.

Supported geometries: all 3D geometries.

Configuration options
---------------------

.. list-table::
  :header-rows: 1

  * - Name
    - Description

  * - ProjectionDataId
    - `Projection data object ID <../concepts.html#data>`_.

  * - ReconstructionDataId
    - `ID of data object <../concepts.html#data>`_ to store the result. The
      content of this data is used as the initial reconstruction.

  * - *option.ReconstructionMaskId*
    - If specified, `data object ID <../concepts.html#data>`_ of a
      volume-data-sized volume to be used as a `mask <../misc.html#masks>`_.

  * - *option.DetectorSuperSampling*
    - During forward projection, each detector pixel will be subdivided by this
      factor along each dimension. This should only be used if detector pixels
      are larger than the voxels in the volume (default: 1).

  * - *option.VoxelSuperSampling*
    - During backprojection, each voxel in the volume will be subdivided by this
      factor along each dimension. This should only be used if voxels in the
      volume are larger than the detector pixels (default: 1).

  * - *option.GPUindex*
    - The index of the GPU to use (default: 0).

Extra features
--------------

CGLS3D_CUDA supports ``astra.algorithm.get_res_norm()`` /
``astra_mex_algorithm('get_res_norm')`` to get the 2-norm of the difference
between the projection data and the projection of the reconstruction. (The
square root of the sum of squares of differences.)
