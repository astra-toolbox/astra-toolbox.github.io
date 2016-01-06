Toolbox concepts
================

Geometry
--------

Most of the operations supported by the ASTRA Toolbox require
you to specify what the geometry of the (real or virtual) experimental
setup you have used or want to simulate is.

This has two parts: a volume geometry, and a projection geometry.
The volume geometry specifies where in 2D or 3D space the sample or phantom
is located. It takes the form of a 2D rectangle or 3D box, and is
usually centered around the origin.

The projection geometry specifies the location and trajectory of the
ray source and detector. The available types of projection geometries are
2D parallel beam, 2D fan beam, 3D parallel beam and 3D cone beam.

Data
----

Toolbox objects are stored in memory separate from Matlab, and have to
be explicitly transferred from/to Matlab matrices.

They come in four varieties: 2D/3D volume data, and 2D/3D projection data.

Data objects are manipulated with the ``astra_mex_data2d``/``astra.data2d`` and ``astra_mex_data3d``/``astra.data3d``
commands. The usage of these commands is illustrated in the available sample
scripts.

Algorithms
----------

To operate on data objects, the ASTRA Toolbox defines a number of algorithms.
These include the basic forward and backward projection operations, and also
various reconstruction algorithms.

To create algorithm objects, use the ``astra_mex_algorithm``/``astra.algorithm`` command. The usage
of this command is illustrated in the available sample scripts.

