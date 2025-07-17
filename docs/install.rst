Installation instructions
=========================

Windows/Linux, using conda for Python packages
----------------------------------------------

Requirements: `conda <https://conda.io/>`__ Python environment, with 64 bit Python 3.9-3.13.

We provide packages for the ASTRA Toolbox in the ``astra-toolbox`` channel for the conda package
manager. We depend on CUDA packages available from the ``nvidia`` channel. To install ASTRA into
the current conda environment, run:

.. code-block:: bash

   conda install -c astra-toolbox -c nvidia astra-toolbox

We also provide development packages between releases occasionally:

.. code-block:: bash

   conda install -c astra-toolbox/label/dev -c nvidia astra-toolbox


Linux, using pip for Python packages
------------------------------------

Requirements: Python environment with 64 bit Python 3.9-3.13.

.. code-block:: bash

   pip install astra-toolbox

Note that, unlike conda packages, we only provide packages built for Linux platform, and only with
a single reasonably recent version of CUDA toolkit. These packages depend on PyPI CUDA distribution
provided by NVIDIA.


Windows, binary
---------------

Download and unpack the `.zip` archive for the desired version from the :doc:`/downloads/index`.
Add the ``mex\`` and ``tools\`` subdirectories to your MATLAB path, or install the Python wheel
using pip. We require the Microsoft Visual Studio 2017 redistributable package. If this is not
already installed on your system, it is included as vc_redist.x64.exe in the ASTRA zip file.


Linux, from source
------------------

Requirements: automake, libtool, g++ (7 or higher), CUDA (11.0 or higher)

Build dependencies can be obtained via the OS package manager or via conda. For example, a conda
environment with a full set of dependencies can be created with:

.. code-block:: bash

   conda create -n astra-build automake libtool gxx_linux-64 cuda-minimal-build libcufft-dev python cython scipy -c conda-forge

You can then do ``conda activate astra-build`` and use ``--with-cuda=$CONDA_PREFIX`` in the build
configuration.

For MATLAB
^^^^^^^^^^

Additional requirements: MATLAB (R2012a or higher)

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

If you want to build the Octave interface instead of the MATLAB interface, specify
``--enable-octave`` instead of ``--with-matlab=...``. The Octave files will be installed into
$HOME/astra/octave . On some Linux distributions building the Astra Octave interface will require
the Octave development package to be installed (e.g., liboctave-dev on Ubuntu).

NB: Each MATLAB version only supports a specific range of g++ versions. Despite this, if you have a
newer g++ and if you get errors related to missing GLIBCXX_3.4.xx symbols, it is often possible to
work around this requirement by deleting the version of libstdc++ supplied by MATLAB in
MATLAB_PATH/bin/glnx86 or MATLAB_PATH/bin/glnxa64 (at your own risk), or setting
``LD_PRELOAD=/usr/lib64/libstdc++.so.6`` (or similar) when starting MATLAB.

For Python
^^^^^^^^^^

Additional requirements: Python (3.x), setuptools, Cython, scipy

.. code-block:: bash

   cd build/linux
   ./autogen.sh   # when building a git version
   ./configure --with-cuda=/usr/local/cuda \
               --with-python \
               --with-install-type=module
   make
   make install

This will install Astra into your current Python environment.

As a C++ library
^^^^^^^^^^^^^^^^

.. code-block:: bash

   cd build/linux
   ./autogen.sh   # when building a git version
   ./configure --with-cuda=/usr/local/cuda
   make
   make install-dev

This will install the Astra library and C++ headers.


macOS, from source
------------------

Use the Homebrew package manager to install libtool, autoconf, automake.

.. code-block:: bash

   cd build/linux
   ./autogen.sh
   CPPFLAGS="-I/usr/local/include" NVCCFLAGS="-I/usr/local/include" ./configure \
     --with-cuda=/usr/local/cuda \
     --with-matlab=/Applications/MATLAB_R2016b.app \
     --prefix=$HOME/astra \
     --with-install-type=module
   make
   make install


Windows, from source using Visual Studio 2017
---------------------------------------------

Requirements: Visual Studio 2017 (full or community), CUDA (11.0 or higher), MATLAB (R2012a or
higher) and/or Python 3.x + setuptools + Cython + scipy.

Using the Visual Studio IDE:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Set the environment variable MATLAB_ROOT to your MATLAB install
   location.
2. Open ``build\msvc\astra_vc14.sln`` in Visual Studio.
3. Select the appropriate solution configuration (typically Release_CUDA|x64).
4. Build the solution.
5. Install by copying AstraCuda64.dll and all .mexw64 files from
   ``build\msvc\bin\x64\Release_CUDA`` and the entire ``matlab\tools`` directory to a
   directory to be added to your MATLAB path.

Using .bat scripts in ``build\msvc``:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Edit build_env.bat and set up the correct library versions and paths.

2. -  For MATLAB: Run build_matlab.bat. The .dll and .mexw64 files will be in
      ``build\msvc\bin\x64\Release_Cuda``.
   -  For Python: Run build_python3.bat. This will produce a Wheel file in ``python\dist``
      directory, which can be installed using pip.


Building conda packages
-----------------------

Linux
^^^^^

Requirements: `podman <https://github.com/containers/podman>`__ and `buildah <https://github.com/containers/buildah>`__.

1. Change to ``astra-toolbox/build/conda`` directory
2. Build container images by running the ``containers/setup*.sh`` scripts
3. Run ``./release.sh``

Windows
^^^^^^^

Requirements: conda-build, git,
`Visual Studio 2017 <https://community.chocolatey.org/packages/visualstudio2017community>`__
with `Build Tools <https://community.chocolatey.org/packages/visualstudio2017buildtools>`__
and `Native Desktop workload <https://community.chocolatey.org/packages/visualstudio2017-workload-nativedesktop>`__,
`Windows SDK version 10.0.22621.2 <https://community.chocolatey.org/packages/windows-sdk-11-version-22H2-all>`__,
CUDA toolkit of desired version(s).

1. Activate conda: ``C:\tools\miniconda3\condabin\activate.bat``
2. Activate VS Build Tools:

.. code-block:: bash

  "C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\vcvarsall.bat" amd64 10.0.22621.0 -vcvars_ver=14.1

3. Change to ``astra-toolbox\build\conda`` directory
4. Build libastra packages, skipping the testing phase:

.. code-block:: bash

   conda build -m libastra\win64_build_config.yaml -c nvidia --no-test libastra

5. Build and test astra-toolbox packages:

.. code-block:: bash

   conda build -m astra-toolbox\win64_build_config.yaml -c nvidia --no-test astra-toolbox

6. Test the previously built libastra packages:

.. code-block:: bash

   conda build -c nvidia --test C:\tools\miniconda3\conda-bld\win-64\libastra*.tar.bz2

Local installation
^^^^^^^^^^^^^^^^^^

The built packages can be installed locally using ``conda install astra-toolbox -c nvidia -c local``.


Testing your installation
-------------------------

To perform a (very) basic test of your ASTRA installation in Python, you can run the following
command:

.. tabs::
  .. group-tab:: Python
    .. code-block:: python

       import astra
       astra.test()

  .. group-tab:: MATLAB
    .. code-block:: matlab

       astra_test
