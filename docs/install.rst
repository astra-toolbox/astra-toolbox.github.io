Installation instructions
=========================

Windows, binary
---------------

Add the mex and tools subdirectories to your MATLAB path, or copy the Python astra module to your Python site-packages directory.

Linux/Windows, using conda for python
-------------------------------------

Requirements: `conda <http://conda.pydata.org/>`_ python environment, with 64 bit Python 2.7 or 3.5.

There are packages available for the ASTRA Toolbox in the astra-toolbox
channel for the conda package manager. To use these, run the following
inside a conda environment.

.. code-block:: bash

  conda install -c astra-toolbox astra-toolbox

Linux, from source
------------------

For Matlab
^^^^^^^^^^

Requirements: g++, boost, CUDA (5.5 or higher), Matlab (R2012a or higher)

.. code-block:: bash

  cd build/linux
  ./autogen.sh   # when building a git version
  ./configure --with-cuda=/usr/local/cuda \
              --with-matlab=/usr/local/MATLAB/R2012a \
              --prefix=$HOME/astra \
              --with-install-type=module
  make
  make install

Add $HOME/astra/matlab and its subdirectories (tools, mex) to your matlab path.

If you want to build the Octave interface instead of the Matlab interface,
specify --enable-octave instead of --with-matlab=... . The Octave files
will be installed into $HOME/astra/octave .


NB: Each matlab version only supports a specific range of g++ versions.
Despite this, if you have a newer g++ and if you get errors related to missing
GLIBCXX_3.4.xx symbols, it is often possible to work around this requirement
by deleting the version of libstdc++ supplied by matlab in
MATLAB_PATH/bin/glnx86 or MATLAB_PATH/bin/glnxa64 (at your own risk),
or setting LD_PRELOAD=/usr/lib64/libstdc++.so.6 (or similar) when starting
matlab.

For Python
^^^^^^^^^^

Requirements: g++, boost, CUDA (5.5 or higher), Python (2.7 or 3.x)

.. code-block:: bash

  cd build/linux
  ./autogen.sh   # when building a git version
  ./configure --with-cuda=/usr/local/cuda \
              --with-python \
              --with-install-type=module
  make
  make install

This will install ASTRA into your current Python environment.


Windows, from source using Visual Studio 2015
---------------------------------------------

Requirements: Visual Studio 2015 (full or community), boost (recent), CUDA 8.0, Matlab (R2012a or higher) and/or WinPython 2.7/3.x.

Using the Visual Studio IDE
^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Set the environment variable MATLAB_ROOT to your matlab install location.
* Copy boost headers to lib\include\boost, and boost libraries to bin\x64.
* Open astra_vc14.sln in Visual Studio.
* Select the appropriate solution configuration (typically Release_CUDA|x64).
* Build the solution.
* Install by copying AstraCuda64.dll and all .mexw64 files from bin\x64\Release_CUDA and the entire matlab/tools directory to a directory to be added to your matlab path.

Using .bat scripts in build\msvc
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Edit build_env.bat and set up the correct directories.
* Run build_setup.bat to automatically copy the boost headers and libraries.
* For matlab: Run build_matlab.bat. The .dll and .mexw64 files will be in bin\x64\Release_Cuda.
* For python 2.7/3.5: Run build_python27.bat or build_python35.bat. ASTRA will be directly installed into site-packages.



Linux, building conda packages
------------------------------

To build your own `conda <http://conda.pydata.org/>`_ packages for the ASTRA toolbox, perform the following steps inside the conda environment:

.. code-block:: bash

  cd python/conda/libastra
  CUDA_ROOT=/path/to/cuda conda-build ./ # Build C++ library
  cd ../
  CUDA_ROOT=/path/to/cuda conda-build ./ # Build Python interface


