FP3D_CUDA
=========

This is a GPU implementation of a simple forward projection algorithm for 3D data sets. It takes a volume as input, and returns the projection data.

Supported geometries: parallel3d, parallel3d_vec, cone, cone_vec.

Configuration options
---------------------

.. list-table::
  :header-rows: 1

  * - Name
    - Description

  * - ProjectionDataId
    - `Data object ID <../concepts.html#data>`_ to store the result. The
      computed forward projection is added to this data.

  * - VolumeDataId
    - `Volume data object ID <../concepts.html#data>`_.

  * - *option.DetectorSuperSampling*
    - Each detector pixel will be subdivided by this factor along each
      dimension. This should only be used if detector pixels are larger than the
      voxels in the volume (default: 1).

  * - *option.GPUindex*
    - The index of the GPU to use (default: 0).
