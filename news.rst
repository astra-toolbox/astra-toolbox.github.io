News and version history
========================

News
----

* 2016-12-07: New version 1.8 released.
  New features:
  * Reduce restrictions on data sizes for 3D GPU FDK reconstructions.
  * Add multi-GPU support for 3D GPU FP, BP, FDK
  * Add support for relaxation factors in SIRT, SART
  * Further reduced restrictions on volume geometries: voxels no longer have to be cubes
  * The Windows release now requires CUDA 8.0, so you may need to update CUDA.
* 2015-12-23: New version 1.7.1beta released. This is a small bugfix release after v1.7beta. ASTRA version 1.7beta contains a few large experimental new features, which is why we have given it the beta tag. 
  If it does not work properly for you, all files for astra-1.6 are also still available for download: astra-1.6 downloads.
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
