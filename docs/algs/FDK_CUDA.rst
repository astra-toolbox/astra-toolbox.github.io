FDK_CUDA
========

.. versionadded:: 2.2
   Add support for flexible filter type configuration. Before, only 'ram-lak' and 'sinorgam' filter types were supported.

This is a GPU implementation of the FDK algorithm for 3D circular cone beam data sets. It takes
projection data as input, and returns the reconstruction.

Supported geometries: cone, cone_vec.

**Note:** cone_vec geometries that significantly deviate from
a circular scanning geometry will likely lead to artifacts in the reconstruction.

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

  * - *option.FilterType*
    - | Type of projection filter. Options:
      | * 'none'
      | * 'ram-lak' (default)
      | * 'shepp-logan'
      | * 'cosine'
      | * 'hamming'
      | * 'hann'
      | * 'tukey'
      | * 'lanczos'
      | * 'triangular'
      | * 'gaussian'
      | * 'barlett-hann'
      | * 'blackman'
      | * 'nuttall'
      | * 'blackman-harris'
      | * 'blackman-nuttall'
      | * 'flat-top'
      | * 'kaiser'
      | * 'parzen'
      | * 'projection' (Fourier space filter, all projection directions share one filter)
      | * 'sinogram' (Fourier space filter, every projection direction has its own filter)
      | * 'rprojection' (real space filter, all projection directions share one filter)
      | * 'rsinogram' (real space filter, every projection direction has its own filter)

  * - *option.FilterParameter*
    - Parameter value for the 'tukey', 'gaussian', 'blackman' and 'kaiser'
      filter types.

  * - *option.FilterD*
    - "D" parameter value for 'shepp-logan', 'cosine', 'hamming' and 'hann'
      filter types.

  * - *option.FilterSinogramId*
    - The `data object ID <../concepts.html#data>`_ of the filter data for
      'projection', 'sinogram', 'rprojection' and 'rsinogram' filter types.

  * - *option.ShortScan*
    - If enabled, do Parker weighting to support non-360-degree data. This needs
      an angle range of at least 180 degrees plus twice the fan angle (default:
      false).

  * - *option.VoxelSuperSampling*
    - Each voxel in the volume will be subdivided by this factor along each
      dimension. This should only be used if voxels in the volume are
      larger than the detector pixels (default: 1).

  * - *option.GPUindex*
    - The index of the GPU to use (default: 0).
