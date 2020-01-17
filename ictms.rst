ASTRA Toolbox workshop at ICTMS 2019
====================================

We will teach an ASTRA workshop on 22 July 2019 at `ICTMS 2019 <http://ictms2019.org/>`_ in Australia.

This workshop will consist of two 90-minute parts which can be attended either separately or together. In the first part, we will mainly consider 2D parallel-beam tomography, with applications in synchrotron tomography and electron tomography. In the second part, we will mainly consider 3D cone-beam tomography, with applications in laboratory micro-CT and medical CBCT.

There will be ample opportunity to ask questions. If you bring a laptop with ASTRA already installed (GPU not necessary), there will be the opportunity to do a few hands-on exercises to get started with using ASTRA at the end of each session.

Registration is required on the `ICTMS Workshops page <http://ictms2019.org/workshop.php>`_.

The slides for the workshop are available here: `2D part <https://www.astra-toolbox.com/files/misc/ICTMS2019/20190722_ICTMS_ASTRA_workshop_2d.pdf>`_, `3D part <https://www.astra-toolbox.com/files/misc/ICTMS2019/20190722_ICTMS_ASTRA_workshop_3d.pdf>`_.


For more information on ASTRA itself, please visit our main webpage https://www.astra-toolbox.com/ and our `GitHub repository <https://github.com/astra-toolbox/astra-toolbox>`_.

Setting up ASTRA on your laptop for the workshop
------------------------------------------------

If you want to participate in the hands-on section of the workshop, we request that you install Python 3 and ASTRA on your laptop beforehand, as time (and internet bandwidth) during the workshop itself is limited. The recommended way is to install `Miniconda <https://docs.conda.io/en/latest/miniconda.html>`_, and then run the following from a terminal:

.. code:: sh

  conda install -c astra-toolbox/label/dev astra-toolbox astra_plot jupyterlab

This will install the latest (development) version of ASTRA, a package ``astra_plot`` which we will use
to visualize ASTRA geometries, and `JupyterLab <https://jupyterlab.readthedocs.io/en/stable/>`_. Start the JupyterLab notebook using the following command, which will then open JupyterLab in your browser.

.. code:: sh

  jupyter lab

To test the installation, create a new Python notebook in Jupyter Lab, and run:

.. code:: python

  import astra
  astra.test()

This should then display output similar to one of the following, depending
on if you have a NVIDIA GPU with CUDA installed or not. Both are sufficient
for participating in the workshop at ICTMS.

::

  Getting GPU info... GPU #0: GeForce GTX 1070, with 8119MB
  Testing basic CPU 2D functionality... Ok
  Testing basic CUDA 2D functionality... Ok
  Testing basic CUDA 3D functionality... Ok

::

  No GPU support available
  Testing basic CPU 2D functionality... Ok

Exercises for the 2D session
----------------------------

#. Run astra.test() to test your installation

#. Download the `2D_sample notebook <https://www.astra-toolbox.com/files/misc/ICTMS2019/2D_sample.ipynb>`_ and the used `phantom image <https://www.astra-toolbox.com/files/misc/ICTMS2019/phantom.mat>`_.

#. Define a parallel beam geometry (not too large, up to 256x256),
   and your favourite phantom image. Use ASTRA to perform a forward projection
   and a backprojection, and display the results.

#. Using the sinogram from the previous exercise, run reconstructions with
   the ``FBP`` and ``SIRT`` algorithms in ASTRA, and display the results.

#. While running ``SIRT``, compute the norm of the projection error after
   each iteration. Also compute the norm the difference with the phantom
   image after each iteration. Plot these two series to visualize the
   convergence. Do the same thing for the ``SART`` algorithm. (NB: For SART,
   each iteration processes only a single projection angle.)

#. Use ``astra.geom_postalignment`` to compute a sinogram of a phantom with
   a non-centered detector. Reconstruct this sinogram as if the detector
   were centered, to visualize the artefacts this creates. Also reconstruct
   it with the correct(ed) geometry to confirm.


Exercises for the 3D session
----------------------------

#. Run astra.test() to test your installation

#. Run the `plot_sample notebook <https://www.astra-toolbox.com/files/misc/ICTMS2019/plot_sample.ipynb>`_ to test your ``astra_plot`` installation.

#. Download the `2D_sample notebook <https://www.astra-toolbox.com/files/misc/ICTMS2019/2D_sample.ipynb>`_ and the used `phantom image <https://www.astra-toolbox.com/files/misc/ICTMS2019/phantom.mat>`_. There's also the `3D_sample notebook <https://www.astra-toolbox.com/files/misc/ICTMS2019/3D_sample.ipynb>`_, but running this requires CUDA.

#. Create (any) cone beam geometry yourself, and plot the geometry.

#. Download `example data <https://www.astra-toolbox.com/files/misc/ICTMS2019/helical.zip>`_, and inspect it. (NB: You can load a ``.mat`` file using ``scipy.io.loadmat`` .) Create a suitable ASTRA projection geometry for this data, and visualize it. If you have CUDA available, also try reconstructing it with ``SIRT``.


Notes
-----

- While the code examples during this workshop will use Python, ASTRA also has a Matlab interface which is very similar to the Python interface, and (almost) all discussed topics also directly apply to the Matlab version.

- If you install ASTRA in a different way, do please also install the ``astra_plot`` package. You can download it manually (for both Matlab and Python) `from our website <https://www.astra-toolbox.com/files/misc/astra_plot_ICTMS_201907.zip>`_.
