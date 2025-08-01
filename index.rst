.. ASTRA Toolbox documentation master file, created by
   sphinx-quickstart on Tue Jan  5 15:10:44 2016.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

The ASTRA Toolbox
=================

.. image:: https://img.shields.io/github/stars/astra-toolbox/astra-toolbox?style=flat&logo=GitHub
   :target: https://github.com/astra-toolbox/astra-toolbox

.. image:: images/conda-downloads-shield.svg
   :target: https://anaconda.org/search?q=astra-toolbox

.. image:: images/pypi-downloads-shield.svg
   :target: https://pypi.org/project/astra-toolbox/

.. image:: images/license-shield.svg

The ASTRA Toolbox is a Python and MATLAB toolbox of high-performance GPU primitives for 2D and 3D tomography.

We support 2D parallel and fan beam geometries, and 3D parallel and cone beam. All of them have highly flexible source/detector positioning.

A large number of 2D and 3D algorithms are available, including FBP, SIRT, SART, CGLS.

The basic forward and backward projection operations are GPU-accelerated, and directly callable from MATLAB and Python to enable building new algorithms.

The source code of the ASTRA Toolbox is available on `GitHub <https://github.com/astra-toolbox/astra-toolbox>`_.

Latest News
-----------

* 4 Aug 2025: `ASTRA v2.4 <news.html#version-2-4>`_ released.

* 16 Apr 2025: `ASTRA v2.3.1 <news.html#version-2-3-1>`_ released.

* 22 Feb 2025: `ASTRA v2.3 <news.html#version-2-3>`_ released.

* 12 Jul 2024: `ASTRA v2.2 <news.html#version-2-2>`_ released.

* 31 Jan 2022: `ASTRA v2.1 <news.html#version-2-1>`_ released. This is the official release of the 2.0.9 development version.

* 23 Dec 2021: There are now conda packages for development version 2.0.9 available in the ``astra-toolbox/label/dev`` conda channel to prepare for 2.1.0 in early 2022. This fixes a rare hang in our CUDA FFT code (and also speeds it up) in FBP_CUDA and FDK_CUDA, fixes GPULink support with unpadded rows, output gray value scaling for short-scan FDK, and conda compatibility issues with the conda-forge and nvidia conda channels.

* 27 Oct 2021: `ASTRA v2.0 <news.html#version-2-0>`_ released. This official release wraps up all the consistency and compatibility improvements from the development releases over the past few years. See the news entry for more information on these gray value output range consistency improvements.

* 17 Jan 2020: In the latest development version of ASTRA (`1.9.9.dev <news.html#version-1-9-9dev-changes-in-output-scaling-of-projectors>`_), we have made some consistency improvements to the grey value output ranges of the FP and BP operations.

* 9 Jul 2019: Windows MATLAB/Python packages of the current `1.9.0.dev <news.html#version-1-9-0-dev>`_ version of ASTRA git master are now available for download below. These require CUDA 9.0+.

* We now have development Python packages available from ASTRA git master on conda. Quick installation instructions: ``conda install -c astra-toolbox/label/dev astra-toolbox`` .

Downloads
---------

Main downloads of the latest official release.


.. include:: downloads/2.4.0/downloads.txt

References
----------

If you use the ASTRA Toolbox for your research, we would appreciate it if you would refer to the following papers:

* W\. van Aarle, W. J. Palenstijn, J. Cant, E. Janssens, F. Bleichrodt, A. Dabravolski, J. De Beenhouwer, K. J. Batenburg, and J. Sijbers, "Fast and Flexible X-ray Tomography Using the ASTRA Toolbox", *Optics Express*, 24(22), 25129-25147, (2016), http://dx.doi.org/10.1364/OE.24.025129

* W\. van Aarle, W. J. Palenstijn, J. De Beenhouwer, T. Altantzis, S. Bals, K. J. Batenburg, and J. Sijbers, "The ASTRA Toolbox: A platform for advanced algorithm development in electron tomography", *Ultramicroscopy*, 157, 35–47, (2015), http://dx.doi.org/10.1016/j.ultramic.2015.05.002

Additionally, if you use parallel beam GPU code, we would appreciate it if you would refer to the following paper:

* W\. J. Palenstijn, K. J. Batenburg, and J. Sijbers, "Performance improvements for iterative electron tomography reconstruction using graphics processing units (GPUs)", *Journal of Structural Biology*, vol. 176, issue 2, pp. 250-253, 2011, http://dx.doi.org/10.1016/j.jsb.2011.07.017

License
-------

The ASTRA Toolbox is open source under the GPLv3 license.

Contact
-------

* General questions and discussions: https://github.com/astra-toolbox/astra-toolbox/discussions

* Bug reports and feature requests: https://github.com/astra-toolbox/astra-toolbox/issues

* Email: astra@astra-toolbox.com

Copyright
---------

2010-2025, iMinds-Vision Lab, University of Antwerp http://visielab.uantwerpen.be/

2014-2025, CWI, Amsterdam  http://www.cwi.nl/


.. toctree::
   :maxdepth: 1
   :hidden:

   news
   downloads/index
   docs/index
   apiref/index
