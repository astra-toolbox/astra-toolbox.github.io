BP3D_CUDA
=========

This is a GPU implementation of a simple backprojection algorithm for 3D data
sets. It takes projection data as input, and returns the backprojection of this
data.

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
      content of this data is overwritten.

  * - *option.VoxelSuperSampling*
    - Each voxel in the volume will be subdivided by this factor along each
      dimension. This should only be used if voxels in the volume are
      larger than the detector pixels (default: 1).

  * - *option.GPUindex*
    - The index of the GPU to use (default: 0).
