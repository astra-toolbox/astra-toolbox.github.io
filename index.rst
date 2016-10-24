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

The source code of the ASTRA Toolbox is available on `GitHub <https://github.com/astra-toolbox/astra-toolbox>`_.

Downloads
---------

Main downloads:

.. include:: downloads/1.7.1beta/downloads.txt

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

email: astra@uantwerpen.be website: http://www.astra-toolbox.com/

Copyright: 2010-2016, iMinds-Vision Lab, University of Antwerp http://visielab.uantwerpen.be/ and 2014-2016, CWI, Amsterdam  http://www.cwi.nl/

Contents
--------

.. toctree::
   :maxdepth: 1

   news
   downloads/index
   docs/index
   apiref/index



Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

