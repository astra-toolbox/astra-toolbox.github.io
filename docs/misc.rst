Miscellaneous
=============

Masks
-----

Various reconstruction algorithms support projection data and reconstruction
volume masks. These behave as follows.

The projection data elements corresponding to locations with SinogramMask
value 0.0 will be ignored during the reconstruction. Similarly,
the reconstruction data elements corresponding to locations with
ReconstructionMask value 0.0 will be ignored during the reconstruction, and
their values will be preserved. (Mostly, see note on constraints below.)

The algorithm will behave as if the rows and columns corresponding to the
masked voxels and projection data elements have been removed from the
projection matrix entirely. In other words, it will iteratively try
to match the projection of the non-masked voxels to the non-masked projection
data elements.

NB: MinConstraint/MaxConstraint will affect even masked voxels.

astra_struct
------------

.. code-block:: matlab

 cfg = astra_struct('NAME');

This is the basic script to create a configuration struct for many astra objects.
The returned struct is usually filled with more options after creating it, and then
passed to astra functions such as

.. code-block:: matlab

 id = astra_mex_algorithm('create', cfg);
 id = astra_mex_projector('create', cfg);

The most common usage is for creating algorithm configuration structs. See the pages
on [2D CPU Algorithms], [2D GPU Algorithms], [3D GPU Algorithms] for available
algorithms, and the pages for the individual algorithms for the options they support.

astra_mex_matrix
----------------

astra_mex_matrix is used to manage sparse matrices. These can be created by the ASTRA toolbox itself to obtain
explicit weight matrices (see [astra_mex_projector]), or you can create them yourself for use with the sparse_matrix projection geometry.

It is a wrapper around the MEX file astra_mex_matrix_c.

astra_mex_matrix contains the following commands:

*    create
*    get
*    get_size
*    store
*    delete
*    clear
*    info

**create**

.. code-block:: matlab

 id = astra_mex_matrix('create', S);

Create an ASTRA sparse matrix object from a Matlab sparse matrix.
get

.. code-block:: matlab

 S = astra_mex_matrix('get', id);

Return an ASTRA sparse matrix object as a Matlab sparse matrix.

**get_size**

.. code-block:: matlab

 s = astra_mex_matrix('get_size', id);

Get the size (rows,columns) of the sparse matrix object.

**store**

.. code-block:: matlab

 astra_mex_matrix('store', id, S)

Store a new Matlab sparse matrix in an ASTRA sparse matrix object.

NB: This does not re-allocate memory: the number of rows and
non-zero entries may not be larger than they were when
the object was first created.

**delete**

.. code-block:: matlab

 astra_mex_matrix('delete', id)

Free a single sparse matrix.

**clear**

.. code-block:: matlab

 astra_mex_matrix('clear')

Free all sparse matrices.

**info**

.. code-block:: matlab

 astra_mex_matrix('info')

Print basic information about all allocated sparse matrix objects.

