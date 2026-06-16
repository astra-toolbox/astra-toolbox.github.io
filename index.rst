The ASTRA Toolbox
=================

.. image:: images/github-stars-shield.svg
   :target: https://github.com/astra-toolbox/astra-toolbox

.. image:: images/conda-downloads-shield.svg
   :target: https://anaconda.org/search?q=astra-toolbox

.. image:: images/pypi-downloads-shield.svg
   :target: https://pypi.org/project/astra-toolbox/

.. image:: images/license-shield.svg

The ASTRA Toolbox is a Python and MATLAB toolbox of high-performance GPU primitives for
2D and 3D tomography. Its features include:

* Support for 2D parallel and fan beam, as well as 3D parallel and cone beam geometries
  with highly flexible source/detector positioning.

* A number of 2D and 3D reconstruction algorithms, including FBP, SIRT, SART, CGLS.

* GPU-accelerated forward and backward projection operations that are directly callable
  from Python and MATLAB to enable building new algorithms.

* Automatic splitting of data when it doesn't fit in GPU memory, and support for using
  multiple GPUs simultaneously.

* Zero-copy (GPU) data exchange with different Python libraries (NumPy, PyTorch, CuPy, JAX).

The source code of the ASTRA Toolbox is available on `GitHub <https://github.com/astra-toolbox/astra-toolbox>`_.


Install
-------

.. tabs::
   .. group-tab:: Python (conda)
       .. code-block:: bash

         conda install -c astra-toolbox -c nvidia astra-toolbox

   .. group-tab:: Python (conda-forge)
      .. code-block:: bash

         conda install -c conda-forge astra-toolbox

   .. group-tab:: Python (pip)
      .. code-block:: bash

         pip install astra-toolbox

   .. group-tab:: MATLAB

      On Windows, we provide precompiled binaries on the `downloads page <downloads/index.html>`_.
      On Linux and macOS, the toolbox  needs to be built from source using the
      `build instructions <docs/install.html#linux-from-source>`_.


Latest News
-----------

2026-06-17: Version 2.5.0
~~~~~~~~~~~~~~~~~~~~~~~~~
* Add support for 2D projectors in DLPack functionality.

  This allows for zero-copy linking of CPU/GPU arrays from Python libraries supporting
  the DLPack standard, including PyTorch, TensorFlow, CuPy and JAX. Previously, this was
  only supported for 3D data.

* Add ``direct_FP`` / ``direct_BP`` to ``astra.projector`` and ``astra.projector3d`` modules.

  This allows for using ASTRA projectors directly on DLPack arrays, without creating an
  ASTRA data or algorithm objects. See `2D Projectors <docs/proj2d.html#direct-fp-direct-bp>`_
  and `3D Projectors <docs/proj3d.html#direct-fp-direct-bp>`_ docs for more information.

* Add experimental support for ROCm/HIP tensors in "direct" and "link" functionality
* Major internal refactoring of data management and CUDA subsystems
* Fix memory leaks in several places
* Fix the crash in fanflat BP_CUDA with more than 2560 angles
* Add PyPI distribution for Windows
* Add support for CUDA 13 and Visual Studio 2022 on Windows
* Add support and distribution for free-threading Python version (Linux only)
* Update set_gpu_index and other GPU selection options for multi-threading contexts
* Fix building on modern macOS

`>>> More news <news.html>`_
~~~~~~~~~~~~~~~~~~~~~~~~~~~~


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
