News and version history
========================

News
----

* 2022-01-31: New version 2.1 released:

 * Improve conda package compatibility with the conda-forge and nvidia channels
 * Fix rare hang in our CUDA FFT code in FBP_CUDA and FDK_CUDA (and speed it up)
 * Fix GPULink support with unpadded rows
 * Fix output scaling in short-scan FDK

* 2021-10-27: New version 2.0 released:

 * Improve output scaling consistency of all projectors. See the 2020-01-17 news entry below.
 * Improve compatibility with modern Python, CUDA, Linux and Windows versions.
 * Add experimental Python interfaces to FP3D, BP3D, FDK to make ASTRA calls by 3rd party toolboxes more efficient and flexible.

* 2020-01-17: Consistency changes in development version of ASTRA

 We have changed the way gray values in forward projections, backprojections and reconstructions scale with the size of detector pixels.  This does not impact geometries where the detector pixels have size 1 in 2D, or 1x1 in 3D.

 In the upcoming ASTRA 2.0, and starting from version 1.9.9.dev, for all geometries and projectors, the value of a pixel in a forward projection is approximately the value of the line integral of the corresponding ray through the volume.

 Of course the interpolation/projection method used will still determine the exact set of pixels contributing to a ray and the exact value as before.

 All backprojectors and reconstruction algorithms have been adapted to match the forward projection.

 Before this change, a decision made in the early pre-1.0 days of ASTRA to make detector values scale linearly with the area of detector pixels led to some in hindsight mathematically unresolvable inconsistencies in behaviour between parallel and fan/cone geometries, and behaviour with oblique projections. The new definition resolves this inherent inconsistency.

 Specific changes:

 * all parallel beam 2D projectors (CPU and CUDA) have been changed as above
 * the 2D fan beam CPU projectors have been changed as above
 * the 2D fan beam CUDA projector did not require changing
 * the 3D CUDA backprojectors have been changed as above. The forward projectors did not require changing
 * the 2D fan beam and 3D cone beam CUDA backprojectors now have an improved approximate match with the respective forward projectors

* 2019-07-09: Development packages of 1.9.0.dev are now available for download.

  The Windows packages now require CUDA 9.0 or higher.
  
* 2016-12-07: New version 1.8 released. New features:

 * Reduce restrictions on data sizes for 3D GPU FDK reconstructions.
 * Add multi-GPU support for 3D GPU FP, BP, FDK
 * Add support for relaxation factors in SIRT, SART
 * Further reduced restrictions on volume geometries: voxels no longer have to be cubes
 * The Windows release now requires CUDA 8.0, so you may need to update CUDA.

* 2015-12-23: New version 1.7.1beta released. This is a small bugfix release after v1.7beta. ASTRA version 1.7beta contains a few large experimental new features, which is why we have given it the beta tag. 
  If it does not work properly for you, all files for astra-1.6 are also still available for download in the Downloads section.
  New features:

 * Experimental MPI distributed computing support in Python. This is only available for Linux+Python, as a separate source download (astra-1.7.1beta_MPI.tar. bz2), or the mpi branch on github.
 * Experimental support in Python for FP and BP of objects composited from multiple 3d data objects, at possibly different resolutions. This also removes some restrictions on data size for 3D GPU FP and BP.
 * Reduced restrictions on volume geometries: The volume no longer has to be centered, and voxels still have to be cubes, but no longer 1x1x1.

* 2015-05-29: New version 1.6 released. The Python interface developed by Daniel Pelt, and the Matlab Spot toolbox operator wrapping ASTRA developed by Folkert Bleichrodt are now integrated into the main ASTRA Toolbox source tree.
* 2015-01-17: From 25 to 27 March, 2015, iMinds-Vision Lab organizes the second ASTRA Toolbox training session. For more info, please contact Wim Van Aarle: wim.vanaarle@uantwerpen.be.
* 2014-02-25: From 9 to 11 April 2014, the iMinds-Vision Lab organizes a training session entitled "Unleashing the ASTRA Tomography Toolbox".
* 2013-07-12: Folkert Bleichrodt from CWI has contributed a wrapper around the ASTRA Toolbox for the Spot toolbox.
* 2013-07-02: New version 1.3 released with some bug fixes and including a version of the DART algorithm by Wim van Aarle from the Vision Lab at the University of Antwerp.
* 2013-04-24: Daniël M. Pelt from CWI has released a Python interface for the ASTRA Toolbox.
* 2012-08-20: ASTRA Toolbox, developed by iMinds-Vision Lab of the University of Antwerp, launched!

Version history
---------------

* 1.9.0.dev

   * add 2D parallel_vec geometry
   * the ExtraDetectorOffset option has been removed. Please use
     parallel_vec to achieve this effect now
   * fix inconsistent rotation direction in CPU fan beam code
   * fix scaling of output values for FDK and fan beam FBP in some geometries
   * fix some restrictions that were limiting 3D data sizes
   * add more filter configuration options for CPU FBP (matching GPU FBP)
   * add astra_test / astra.test() functions to test basic CPU/GPU functionality
   * enable use of the cone_vec geometry for FDK_CUDA. NB: This lets you do
     things that are not mathematically sensible, and should only be used for
     geometries that are effectively circular cone beam geometries.     
   * compatibility fixes for new Windows, Linux, CUDA versions

* 1.8.3 (2017-11-06)

   * fix geometry memory leak in 3D FP/BP
   * fix FDK short scan weighting
   * add preliminary support for building on macOS
   * add experimental support for using externally managed GPU memory from python
     (see samples/python/s021_pygpu.py)
   * our Linux conda python packages now have variants depending on the
     cudatoolkit version
   * add basic post-install tests test_CUDA/test_noCUDA (see README)

* 1.8 (2016-12-05)

   * the Windows binary release now requires CUDA 8.0
   * major changes to the way 'make install' works when building from source
   * removed GPU memory size restrictions for FDK
   * added multi-GPU support to 3D FP/BP/FDK
   * added relaxation factor option to SIRT, SART
   * fixed certain projections parallel to XZ or YZ planes
   * fixed accumulating multiple raylengths in SART
   * for matlab OpTomo, make output type match input type
   * for python OpTomo, add FP/BP functions with optional 'out' argument
   * fixed problems with non-US locales

* 1.7.1beta (2015-12-23)

   * NB: This release has a beta tag as it contains two new
     big experimental features.
   * fix crash with certain 2D CUDA FP calls

* 1.7beta (2015-12-04)

   * NB: This release has a beta tag as it contains two new
     big experimental features.
   * experimental MPI distributed computing support in Python
   * experimental support in Python for FP and BP of objects
     composited from multiple 3d data objects, at possibly different resolutions.
     This also removes some restrictions on data size for 3D GPU FP and BP.
   * support for Python algorithm plugins
   * removed restrictions on volume geometries:

   * The volume no longer has to be centered.
   * Voxels still have to be cubes, but no longer 1x1x1.
   * build fixes for newer platforms
   * various consistency and bug fixes

* 1.6 (2015-05-29)

   * integrate and improve python interface
   * integrate opSpot-based opTomo operator
   * build fixes for newer platforms
   * various consistency and bug fixes

* 1.5 (2015-01-30)

   * add support for fan beam FBP
   * remove limits on number of angles in GPU code
     (They are still limited by available memory, however)
   * update the included version of the DART algorithm
   * build fixes for newer platforms
   * various consistency and bug fixes

* 1.4 (2014-04-07)

   * various consistency and bug fixes
   * add global astra_set_gpu_index

* 1.3 (2013-07-02)

   * various consistency and bug fixes
* add a version of the DART algorithm (written by Wim van Aarle)

* 1.2 (2013-03-01)

   * various consistency and bug fixes

* 1.1 (2012-10-24)

   * add support for matlab single arrays in mex interface

* 1.0 (2012-08-22)

   * first public release
