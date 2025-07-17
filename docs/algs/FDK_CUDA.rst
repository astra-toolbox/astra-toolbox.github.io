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
+-------------------------------+----------+-----------------------------------------------------------+
| name                          | type     | description                                               |
+===============================+==========+===========================================================+
| cfg.ProjectionDataId          | required | The astra_mex_data3d ID of the projection data.           |
+-------------------------------+----------+-----------------------------------------------------------+
| cfg.ReconstructionDataId      | required | The astra_mex_data3d ID of the reconstruction data.       |
|                               |          |                                                           |
|                               |          | The content of this is overwritten.                       |
+-------------------------------+----------+-----------------------------------------------------------+
| cfg.FilterType                | optional | | Type of projection filter. Options:                     |
|                               |          | | * 'none'                                                |
|                               |          | | * 'ram-lak' (default)                                   |
|                               |          | | * 'shepp-logan'                                         |
|                               |          | | * 'cosine'                                              |
|                               |          | | * 'hamming'                                             |
|                               |          | | * 'hann'                                                |
|                               |          | | * 'tukey'                                               |
|                               |          | | * 'lanczos'                                             |
|                               |          | | * 'triangular'                                          |
|                               |          | | * 'gaussian'                                            |
|                               |          | | * 'barlett-hann'                                        |
|                               |          | | * 'blackman'                                            |
|                               |          | | * 'nuttall'                                             |
|                               |          | | * 'blackman-harris'                                     |
|                               |          | | * 'blackman-nuttall'                                    |
|                               |          | | * 'flat-top'                                            |
|                               |          | | * 'kaiser'                                              |
|                               |          | | * 'parzen'                                              |
|                               |          | | * 'projection' (Fourier space filter, all projection    |
|                               |          | | directions share one filter)                            |
|                               |          | | * 'sinogram' (Fourier space filter, every projection    |
|                               |          | | direction has its own filter)                           |
|                               |          | | * 'rprojection' (real space filter, all projection      |
|                               |          | | directions share one filter)                            |
|                               |          | | * 'rsinogram' (real space filter, every projection      |
|                               |          | | direction has its own filter)                           |
+-------------------------------+----------+-----------------------------------------------------------+
| cfg.FilterSinogramId          | optional | The astra_mex_data2d ID of the filter data for            |
|                               |          |                                                           |
|                               |          | 'projection', 'sinogram', 'rprojection'  and              |
|                               |          |                                                           |
|                               |          | 'rsinogram' filter types.                                 |
+-------------------------------+----------+-----------------------------------------------------------+
| cfg.FilterParameter           | optional | Parameter value for the 'tukey', 'gaussian',              |
|                               |          |                                                           |
|                               |          | 'blackman' and 'kaiser' filter types.                     |
+-------------------------------+----------+-----------------------------------------------------------+
| cfg.FilterD                   | optional | "D" parameter value for 'shepp-logan', 'cosine',          |
|                               |          |                                                           |
|                               |          | 'hamming' and 'hann'  filter types.                       |
+-------------------------------+----------+-----------------------------------------------------------+
| cfg.option.ShortScan          | optional | If enabled, do Parker weighting to support non-360-degree |
|                               |          |                                                           |
|                               |          | data. This needs an angle range of at least 180 degrees   |
|                               |          |                                                           |
|                               |          | plus twice the cone angle. (default: false)               |
+-------------------------------+----------+-----------------------------------------------------------+
| cfg.option.VoxelSuperSampling | optional | For the backward projection, VoxelSuperSampling^3 rays    |
|                               |          |                                                           |
|                               |          | will be used. This should only be used if voxels in the   |
|                               |          |                                                           |
|                               |          | reconstruction volume are larger than the detector        |
|                               |          |                                                           |
|                               |          | pixels. (default: 1)                                      |
+-------------------------------+----------+-----------------------------------------------------------+
| cfg.option.GPUindex           | optional | The index (zero-based) of the GPU to use. (default: 0)    |
+-------------------------------+----------+-----------------------------------------------------------+
