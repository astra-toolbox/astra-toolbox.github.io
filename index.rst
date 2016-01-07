.. ASTRA Toolbox documentation master file, created by
   sphinx-quickstart on Tue Jan  5 15:10:44 2016.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

The ASTRA Toolbox
=================

The ASTRA Toolbox is a MATLAB and Python toolbox of high-performance GPU primitives for 2D and 3D tomography.

We support 2D parallel and fan beam geometries, and 3D parallel and cone beam. All of them have highly flexible source/detector positioning.

A large number of 2D and 3D algorithms are available, including FBP, SIRT, SART, CGLS.

The basic forward and backward projection operations are GPU-accelerated, and directly callable from MATLAB and Python to enable building new algorithms.

Downloads
---------

Main downloads:

* ASTRA v1.7.1beta for Windows 64 bit CUDA/MATLAB: `astra-1.7.1beta-matlab-win-x64.zip <http://sourceforge.net/projects/astra-toolbox/files/astra-1.7.1beta/astra-1.7.1beta-matlab-win-x64.zip/download>`_
* ASTRA v1.7.1beta for Windows 64 bit CUDA/Python 2.7: `astra-1.7.1beta-python27-win-x64.zip <http://sourceforge.net/projects/astra-toolbox/files/astra-1.7.1beta/astra-1.7.1beta-python27-win-x64.zip/download>`_
* ASTRA v1.7.1beta for Windows 64 bit CUDA/Python 3.4: `astra-1.7.1beta-python34-win-x64.zip <http://sourceforge.net/projects/astra-toolbox/files/astra-1.7.1beta/astra-1.7.1beta-python34-win-x64.zip/download>`_
* ASTRA v1.7.1beta Source Code for Linux: `astra-1.7.1beta.tar.bz2 <http://sourceforge.net/projects/astra-toolbox/files/astra-1.7.1beta/astra-1.7.1beta.tar.bz2/download>`_
* ASTRA v1.7.1beta Source Code for Linux, with MPI support: `astra-1.7.1beta_MPI.tar.bz2 <http://sourceforge.net/projects/astra-toolbox/files/astra-1.7.1beta/astra-1.7.1beta_MPI.tar.bz2/download>`_

For compiling on Windows we only provide Visual Studio 2008 and 2012 project files. We have also packaged a set of external libraries and headers for all the build dependencies:

* ASTRA v1.7.1beta Source Code for Windows/VS2008/2012: `astra-1.7.1beta.zip <http://sourceforge.net/projects/astra-toolbox/files/astra-1.7.1beta/astra-1.7.1beta.zip/download>`_
* ASTRA v1.7.1beta Compilation Support files for Windows/VS2008/2012: `astra-1.7.1beta-windows_build_support.zip <http://sourceforge.net/projects/astra-toolbox/files/astra-1.7.1beta/astra-1.7.1beta-windows_build_support.zip/download>`_


References
----------

If you use the ASTRA Toolbox for your research, we would appreciate it if you would refer to the following paper:

* W\. van Aarle, W. J. Palenstijn, J. De Beenhouwer, T. Altantzis, S. Bals, K. J. Batenburg, and J. Sijbers, "The ASTRA Toolbox: A platform for advanced algorithm development in electron tomography", *Ultramicroscopy* (2015), http://dx.doi.org/10.1016/j.ultramic.2015.05.002

Additionally, if you use parallel beam GPU code, we would appreciate it if you would refer to the following paper:

* W\. J. Palenstijn, K J. Batenburg, and J. Sijbers, "Performance improvements for iterative electron tomography reconstruction using graphics processing units (GPUs)", *Journal of Structural Biology*, vol. 176, issue 2, pp. 250-253, 2011, http://dx.doi.org/10.1016/j.jsb.2011.07.017

License
-------

The ASTRA Toolbox is open source under the GPLv3 license.

Contact
-------

email: astra@uantwerpen.be website: https://astra-toolbox.readthedocs.org

Copyright: 2010-2015, iMinds-Vision Lab, University of Antwerp 2014-2015, CWI, Amsterdam http://visielab.uantwerpen.be/ and http://www.cwi.nl/

Contents
--------

.. toctree::
   :maxdepth: 1

   news
   docs/index
   apiref/index



Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

