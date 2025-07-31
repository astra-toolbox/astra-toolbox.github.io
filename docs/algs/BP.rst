BP
==

This is a CPU implementation of a simple backprojection algorithm for 2D data sets. It takes a projector and projection data as input, and returns the backprojection of this data.

Supported geometries: parallel, parallel_vec, fanflat, fanflat_vec, matrix.

Configuration options
---------------------

.. list-table::
  :header-rows: 1

  * - Name
    - Description

  * - ProjectorId
    - `Projector object ID <../proj2d.html>`_.

  * - ProjectionDataId
    - `Projection data object ID <../concepts.html#data>`_.

  * - ReconstructionDataId
    - `ID of data object <../concepts.html#data>`_ to store the result. The
      content of this data is overwritten.

  * - *option.SinogramMaskId*
    - If specified, `data object ID <../concepts.html#data>`_ of a
      projection-data-sized volume to be used as a `mask <../misc.html#masks>`_.

  * - *option.ReconstructionMaskId*
    - If specified, `data object ID <../concepts.html#data>`_ of a
      volume-data-sized volume to be used as a `mask <../misc.html#masks>`_.
