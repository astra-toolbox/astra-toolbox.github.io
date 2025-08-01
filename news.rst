News
====

2025-08-04: Version 2.4
-----------------------
* Add support for curved detectors (cyl_cone_vec 3D geometry type).
* Add initial experimental support for building with AMD HIP instead of CUDA.
* Improve performance of automatic splitting for FDK/FP3D/BP3D when data doesn't fit in GPU memory
* Add conda packages for CUDA 12.9
* Fix non-deterministic results when FBP_CUDA was used with fanflat_vec geometries (e.g. when
  post-alignment was applied).
* Fix crash when trying to delete linked 3D data in MATLAB.
* Fix Python samples.
* Modernize Python build system.

2025-04-16: Version 2.3.1
-------------------------
* Fix GPULink missing in astra.data3d module
* Fix crash in MATLAB when calling astra_mex_direct
* Fix missing accelerated virtual GPU code for CUDA architectures > 8.6 in Linux packages

2025-02-22: Version 2.3
-----------------------
* Add support for zero-copy linking of GPU/CPU arrays from Python libraries supporting the DLPack
  standard, including PyTorch, TensorFlow, CuPy and JAX. See ``astra.data3d.link`` documentation
  in :doc:`docs/data3d` for more information. Note that linking of 2D data is still only
  implemented for NumPy arrays.
* Add support for using different filter types in :doc:`docs/algs/FDK_CUDA`. The filter
  configuration for FDK_CUDA now matches that for FBP/FBP_CUDA.
* Add conda packages for Python 3.13, CUDA 12.6, 12.8, and NumPy 2.
* Add automatic asynchronous execution to ``astra.experimental`` functions in Python.
* Add automatic splitting of FDK execution along angular dimension, allowing for larger maximum
  number of projections.
* Add error reporting in ``astra.algorithm.run`` function.
* Add an extensive Python unit test suite.
* Fix incorrect results with GPUs with more than 48GB of memory.
* Fix multiple minor bugs.
* Require C++17, raising minimum system requirements to CUDA 11.
* Deprecate Python 3.7 conda packages.
* Remove boost and six dependencies.

2024-07-12: Version 2.2
-----------------------
* Add support for Python 3.12, CUDA 12.5, and NumPy 2.0
* ASTRA conda packages for CUDA 11.6 and above now depend on Nvidia's
  modular CUDA packages instead of on the monolithic cudatoolkit package.
  This improves compatibility of our conda packages with PyTorch and reduces
  the installation size.
* Speed up FDK, especially when using multiple GPUs
* Improve error reporting
* Add Shepp-Logan phantom generator.
* Update the build infrastructure and instructions.

2022-01-31: Version 2.1
-----------------------
* Improve conda package compatibility with the conda-forge and nvidia channels
* Fix rare hang in our CUDA FFT code in FBP_CUDA and FDK_CUDA (and speed it up)
* Fix GPULink support with unpadded rows
* Fix output scaling in short-scan FDK

2021-10-27: Version 2.0
-----------------------
* Improve output scaling consistency of all projectors. See the 2020-01-17 news entry below.
* Improve compatibility with modern Python, CUDA, Linux and Windows versions.
* Add experimental Python interfaces to FP3D, BP3D, FDK to make ASTRA calls by 3rd party toolboxes
  more efficient and flexible.

2020-01-17: Version 1.9.9dev: Changes in output scaling of projectors
---------------------------------------------------------------------
We have changed the way gray values in forward projections, backprojections and reconstructions
scale with the size of detector pixels.  This does not impact geometries where the detector pixels
have size 1 in 2D, or 1x1 in 3D.

In the upcoming ASTRA 2.0, and starting from version 1.9.9.dev, for all geometries and projectors,
the value of a pixel in a forward projection is approximately the value of the line integral of
the corresponding ray through the volume.

Of course the interpolation/projection method used will still determine the exact set of pixels
contributing to a ray and the exact value as before.

All backprojectors and reconstruction algorithms have been adapted to match the forward projection.

Before this change, a decision made in the early pre-1.0 days of ASTRA to make detector values
scale linearly with the area of detector pixels led to some in hindsight mathematically
unresolvable inconsistencies in behaviour between parallel and fan/cone geometries, and behaviour
with oblique projections. The new definition resolves this inherent inconsistency.

Specific changes:

* all parallel beam 2D projectors (CPU and CUDA) have been changed as above
* the 2D fan beam CPU projectors have been changed as above
* the 2D fan beam CUDA projector did not require changing
* the 3D CUDA backprojectors have been changed as above. The forward projectors did not require changing
* the 2D fan beam and 3D cone beam CUDA backprojectors now have an improved approximate match with the respective forward projectors

2019-07-09: Version 1.9.0.dev
-----------------------------
* Add 2D parallel_vec geometry
* The ExtraDetectorOffset option has been removed. Please use
  parallel_vec to achieve this effect now
* Fix inconsistent rotation direction in CPU fan beam code
* Fix scaling of output values for FDK and fan beam FBP in some geometries
* Fix some restrictions that were limiting 3D data sizes
* Add more filter configuration options for CPU FBP (matching GPU FBP)
* Add astra_test / astra.test() functions to test basic CPU/GPU functionality
* Enable use of the cone_vec geometry for FDK_CUDA. NB: This lets you do
  things that are not mathematically sensible, and should only be used for
  geometries that are effectively circular cone beam geometries.
* Compatibility fixes for new Windows, Linux, CUDA versions
* Windows packages now require CUDA 9.0 or higher

2017-11-06: Version 1.8.3
-------------------------
* Fix geometry memory leak in 3D FP/BP
* Fix FDK short scan weighting
* Add preliminary support for building on macOS
* Add experimental support for using externally managed GPU memory from Python
  (see samples/python/s021_pygpu.py)
* Our Linux conda Python packages now have variants depending on the
  cudatoolkit version
* Add basic post-install tests test_CUDA/test_noCUDA (see README)

2016-12-05: Version 1.8
-----------------------
* Remove GPU memory size restrictions for FDK
* Add multi-GPU support to 3D FP/BP/FDK
* Add relaxation factor option to SIRT, SART
* Add support for non-cubic voxels in volume geometry
* Fix certain projections parallel to XZ or YZ planes
* Fix accumulating multiple raylengths in SART
* For MATLAB OpTomo, make output type match input type
* For Python OpTomo, add FP/BP functions with optional 'out' argument
* Fix problems with non-US locales
* Windows binary release now requires CUDA 8.0
* Major changes to the way 'make install' works when building from source

2015-12-23: Version 1.7.1beta
-----------------------------
* Fix crash with certain 2D CUDA FP calls

2015-12-04: Version 1.7beta
---------------------------
* NB: This release has a beta tag as it contains two new
  big experimental features.
* Experimental MPI distributed computing support in Python
* Experimental support in Python for FP and BP of objects
  composited from multiple 3d data objects, at possibly different resolutions.
  This also removes some restrictions on data size for 3D GPU FP and BP.
* Add support for Python algorithm plugins
* Remove restrictions on volume geometries:
  * The volume no longer has to be centered.
  * Voxels still have to be cubes, but no longer 1x1x1.
* Fix building on newer platforms
* Various consistency and bug fixes

2015-05-29: Version 1.6
-----------------------
* Integrate and improve Python interface
* Integrate opSpot-based opTomo operator
* Build fixes for newer platforms
* Various consistency and bug fixes

2015-01-30: Version 1.5
-----------------------
* Add support for fan beam FBP
* Remove limits on number of angles in GPU code
  (They are still limited by available memory, however)
* Update the included version of the DART algorithm
* Build fixes for newer platforms
* Various consistency and bug fixes

2015-01-17: ASTRA Toolbox training session
------------------------------------------

From 25 to 27 March, 2015, iMinds-Vision Lab organizes the second ASTRA Toolbox training session. For more info, please contact Wim Van Aarle: wim.vanaarle@uantwerpen.be.

2014-04-07: Version 1.4
-----------------------
* Various consistency and bug fixes
* Add global astra_set_gpu_index

2014-02-25: Unleashing the ASTRA Tomography Toolbox
---------------------------------------------------
From 9 to 11 April 2014, the iMinds-Vision Lab organizes a training session entitled "Unleashing the ASTRA Tomography Toolbox".

2013-07-12: ASTRA wrapper for Spot toolbox
------------------------------------------
Folkert Bleichrodt from CWI has contributed a wrapper around the ASTRA Toolbox for the Spot toolbox.

2013-07-02: Version 1.3
-----------------------
* Add a version of the DART algorithm (written by Wim van Aarle)
* Various consistency and bug fixes

2013-04-24: Python interface
----------------------------
Daniël M. Pelt from CWI has released a Python interface for the ASTRA Toolbox.

2013-03-01: Version 1.2
-----------------------
* Various consistency and bug fixes

2012-10-24: Version 1.1
-----------------------
* Add support for MATLAB single arrays in mex interface

2012-08-22: Version 1.0
-----------------------
First public release.

2012-08-20: Launch
------------------
ASTRA Toolbox, developed by iMinds-Vision Lab of the University of Antwerp, launched!
