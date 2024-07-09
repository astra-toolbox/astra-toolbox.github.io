Installation instructions
=========================

Linux/Windows, using conda for Python
-------------------------------------

Requirements: `conda <https://docs.conda.io/en/latest/>`_ Python environment, with 64 bit Python 3.9-3.12.

Conda packages for ASTRA Toolbox are available from the ``astra-toolbox`` channel, whereas CUDA-related packages can be installed from ``nvidia`` channel. To install ASTRA into the desired conda environment, run:

.. code-block:: bash

  conda install astra-toolbox -c astra-toolbox -c nvidia

Windows, binary
---------------

Add the ``mex/`` and ``tools/`` subdirectories to your MATLAB path, or install
the Python wheel using pip. We require the Microsoft Visual Studio 2017
redistributable package. If this is not already installed on your system, it is
included as vc_redist.x64.exe in the ASTRA zip file.

Linux, from source
------------------

For MATLAB
^^^^^^^^^^

Requirements: g++, CUDA (10.2 or higher), MATLAB (R2012a or higher)

.. code-block:: bash

  cd build/linux
  ./autogen.sh   # when building a git version
  ./configure --with-cuda=/usr/local/cuda \
              --with-matlab=/usr/local/MATLAB/R2012a \
              --prefix=$HOME/astra \
              --with-install-type=module
  make
  make install

Add $HOME/astra/matlab and its subdirectories (tools, mex) to your MATLAB path.

If you want to build the Octave interface instead of the MATLAB interface,
specify ``--enable-octave`` instead of ``--with-matlab=...``. The Octave files
will be installed into $HOME/astra/octave .


NB: Each MATLAB version only supports a specific range of g++ versions.
Despite this, if you have a newer g++ and if you get errors related to missing
GLIBCXX_3.4.xx symbols, it is often possible to work around this requirement
by deleting the version of libstdc++ supplied by MATLAB in
MATLAB_PATH/bin/glnx86 or MATLAB_PATH/bin/glnxa64 (at your own risk),
or setting ``LD_PRELOAD=/usr/lib64/libstdc++.so.6`` (or similar) when starting
MATLAB.

For Python
^^^^^^^^^^

Requirements: g++, CUDA (10.2 or higher), Python (3.x), Cython, six, scipy

.. code-block:: bash

  cd build/linux
  ./autogen.sh   # when building a git version
  ./configure --with-cuda=/usr/local/cuda \
              --with-python \
              --with-install-type=module
  make
  make install

This will install ASTRA into your current Python environment.


Windows, from source using Visual Studio 2017
---------------------------------------------

Requirements: Visual Studio 2017 (full or community), boost (recent), CUDA (10.2 or higher), MATLAB (R2012a or higher) and/or WinPython 3.x.

Using the Visual Studio IDE
^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Set the environment variable MATLAB_ROOT to your MATLAB install location.
* Copy boost headers to lib\\include\\boost (i.e., copy the boost subdirectory from the boost source archive to lib\\include), and boost libraries to lib\\x64.
* Open astra_vc14.sln in Visual Studio.
* Select the appropriate solution configuration (typically Release_CUDA|x64).
* Build the solution.
* Install by copying AstraCuda64.dll and all .mexw64 files from bin\\x64\\Release_CUDA and the entire matlab\\tools directory to a directory to be added to your MATLAB path.

Using .bat scripts in build\\msvc
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Edit build_env.bat and set up the correct directories.
* Run build_setup.bat to automatically copy the boost headers and libraries.
* For MATLAB: Run build_matlab.bat. The .dll and .mexw64 files will be in bin\\x64\\Release_Cuda.
* For Python 3.12: Run build_python312.bat. ASTRA will be directly installed into site-packages.



Linux, building conda packages
------------------------------

To build your own `conda <https://docs.conda.io/en/latest/>`_ packages for the ASTRA toolbox, perform the following steps inside the conda environment:

.. code-block:: bash

  cd build/conda/libastra
  CUDA_ROOT=/path/to/cuda conda-build --no-test ./ # Build C++ library
  cd ../astra-toolbox
  CUDA_ROOT=/path/to/cuda conda-build ./ # Build Python interface

