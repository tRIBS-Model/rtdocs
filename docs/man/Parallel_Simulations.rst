Parallel Simulations
====================

tRIBS can distribute a simulation across multiple processors by dividing the watershed into groups of stream reaches, one group per processor. Each processor integrates the hydrology of its own nodes and exchanges water at the boundaries between groups at every timestep. How the domain is divided therefore sets both how evenly the work is shared and how much communication the run has to pay for, and it is the main thing a user controls when running in parallel.

This page covers how to launch a parallel run, how tRIBS partitions the domain, how to choose among the available partitioning methods, and how to inspect a partition before committing to a long simulation. The keyword definitions themselves live in :doc:`Model Input File`; the partition file format is documented in :doc:`Model_Input_Formats`.

What Changed in v6.0.0
----------------------

Parallel tRIBS is now self-contained. Earlier versions required a separate toolchain before a parallel run could start: build the standalone MeshBuilder utility, run it to export reach connectivity, then drive ``gpmetis`` through a chain of Perl scripts (``connectivity2metis.pl`` → ``gpmetis`` → ``metis2tribs.pl``) to produce a ``.reach`` graph file for tRIBS to read. 

As of v6.0.0, METIS (v5.2, with GKlib) is vendored into the tRIBS source tree and compiled into the parallel binary, and the reach graph is built and partitioned in-process at startup. There is nothing to install, nothing to run beforehand, and no graph file to manage unless you want one.

Three changes follow from this, and users upgrading from v5.x should read them before reusing an old input file:

    * *GRAPHOPTION* has been **repurposed**. It previously selected the type of graph file to read; it now selects the partitioning method.
    * *GRAPHFILE* is now **optional**. Left blank, the partition is generated fresh on every run and written alongside the model output.
    * *PARALLELMODE* gains a third value, ``2``, which partitions the domain, reports on the result, and exits without continuing the simulation.

Launching a Parallel Run
------------------------

Parallel runs require the parallel executable ``tRIBSpar``, built by configuring with ``-Dparallel=ON`` (see :doc:`Model_Execution`), and an MPI launcher:

.. code-block:: bash

    mpirun -n 4 ./tRIBSpar inputfile.in

with *PARALLELMODE = 1* set in the input file. **The number of processes passed to** ``mpirun`` **also sets the number of partitions.** There is no input keyword for the processor count; changing ``-n`` is sufficient to repartition the domain, and no input files need to be regenerated.

Requesting *PARALLELMODE = 1* or ``2`` from the serial ``tRIBS`` executable exits with a message.

How the Domain Is Partitioned
-----------------------------

The unit of decomposition is the stream reach, not the individual mesh node. After the mesh and stream network are built, tRIBS constructs a weighted, undirected graph in which each vertex is a reach and each vertex weight is the number of mesh nodes belonging to that reach. Weighting by node count is what makes the partitioner balance computational work rather than merely counting reaches, which vary enormously in size.

Two kinds of edges can connect reaches in this graph:

    * **Channel connections**, the downstream links of the stream network. With *N* reaches there are *N-1* of these. They are always included.
    * **Subsurface neighbors**, pairs of reaches that exchange lateral subsurface flux because some mesh edge has its two endpoints in different reaches. There are typically several times more of these than channel connections.

Every edge that ends up spanning two partitions becomes an MPI exchange performed each timestep, so the partitioner is being asked to cut as few of them as possible while keeping node counts even. Because reaches joined end-to-end are also mesh neighbors, the channel connections are a subset of the subsurface neighbors rather than a separate set.

The graph is partitioned into exactly as many parts as there are MPI processes, and the result is written as a reach partition file.

Choosing a Partitioning Method
------------------------------

The *GRAPHOPTION* keyword selects which edges the partitioner is given and what it is asked to balance. Values outside 0–2 are a fatal error caught during input preprocessing.

    .. tabularcolumns:: |c|c|l|

    +---------------+--------+----------------------------------------------------------------------+
    | *GRAPHOPTION* | Method | Graph given to the partitioner                                       |
    +===============+========+======================================================================+
    | 0             | SF     | Channel connections only.                                            |
    +---------------+--------+----------------------------------------------------------------------+
    | 1             | SSF    | Channel connections plus subsurface neighbor edges.                  |
    +---------------+--------+----------------------------------------------------------------------+
    | 2             | SSFH   | As SSF, plus a second constraint balancing non-headwater reaches     |
    |               |        | across partitions.                                                   |
    +---------------+--------+----------------------------------------------------------------------+

Each method buys one thing at the expense of another, and which trade is favorable depends on the mesh. Running the Big Spring benchmark (63 reaches, 3748 nodes) on nine processors with each method gives:

    .. tabularcolumns:: |c|c|c|c|

    +--------+-------------------+----------------------+-------------------+
    | Method | Channel crossings | Subsurface crossings | Node load balance |
    +========+===================+======================+===================+
    | SF     | 14                | 63                   | 1.119             |
    +--------+-------------------+----------------------+-------------------+
    | SSF    | 19                | 52                   | 1.407             |
    +--------+-------------------+----------------------+-------------------+
    | SSFH   | 31                | 75                   | 1.141             |
    +--------+-------------------+----------------------+-------------------+

SSF does what it is designed to do, it has the fewest subsurface crossings of the three, but on this basin it pays for that with a worse channel cut and a load balance of 1.407, meaning the busiest processor carries roughly 40% more nodes than an even share and the rest wait on it. SSFH's second constraint is active: non-headwater reaches come out at three to four per partition, against a spread of zero to twelve under SSF, and node balance recovers to 1.141. SF, which asks the least of the partitioner, gives the best overall partition here.

The general guidance is to start with SF, and to treat SSF and SSFH as alternatives worth testing rather than upgrades. On a basin where subsurface exchange dominates the communication cost, SSF or SSFH may be the better options. Since a partition can be generated and evaluated in seconds, there is little reason to guess.

Partition-Only Mode
-------------------

Setting *PARALLELMODE = 2* builds the mesh and stream network, partitions the domain, writes the reach partition file, prints statistics, and exits cleanly before any hydrologic objects are constructed. No simulation is performed and no forcing data is read. This makes it practical to compare all three methods, or several processor counts, in the time it would take to start one real run:

.. code-block:: bash

    mpirun -n 4 ./tRIBSpar inputfile.in

with *PARALLELMODE = 2* and the *GRAPHOPTION* value under test. A typical report looks like this:

::

    Part 3b: Creating Stream Reach partitioning
    -----------------------------------------------

    Creating stream reach connectivity table...

    Partitioning stream reach graph...

    No parallel partitioning graph file (GRAPHFILE) provided.
    Generating the partition in-process (SSF, 4 partitions) and writing it to 'results/test/parallel/bigsp_SSF_4nodes.reach'.
    Set GRAPHFILE to this path to reuse it in future runs.
    Partitioned 63 reaches into 4 partitions (SSF); 26 of 158 subsurface neighbor pairs cross processor boundaries
    Partition 0: 937 nodes
    Partition 1: 928 nodes
    Partition 2: 929 nodes
    Partition 3: 954 nodes

    Partition statistics
    --------------------
    Reaches: 63   Nodes: 3748   Partitions: 4

    Channel connections (reach to downstream reach): 62, of which 9 cross processors
    Subsurface neighbors (reach pairs exchanging lateral flux): 158, of which 26 cross processors   <- minimized by SSF
    Reaches joined end-to-end are mesh neighbors too, so the 9 channel crossings are
    counted among the 26 subsurface ones, not additional to them.
    Every processor crossing becomes an MPI exchange each timestep; fewer is better.

     Partition    Reaches   Headwaters      Nodes    Node%
             0         29           14        937    25.0%
             1         11            4        928    24.8%
             2          9            6        929    24.8%
             3         14            8        954    25.5%

    Load balance (largest partition / even split): 1.018 (1.0 = every partition equal)
    Reach sizes (nodes): smallest 1, median 26, largest 307
    No single reach exceeds an even share, so any remaining imbalance comes from how
    whole reaches combine and from keeping cross processor communication low.

If *GRAPHFILE* is set, partition-only mode validates that file against the current mesh and processor count and reports its statistics instead of generating a new partition. This is a quick way to confirm that an archived partition is still usable before submitting a job that depends on it.

Reading the Partition Statistics
--------------------------------

Three numbers carry most of the signal.

**Load balance** is the largest partition's node count divided by an even share. A value of 1.0 means every partition is equal; 1.018, as above, means the busiest processor carries under 2% more than its share. Because processors synchronize each timestep, the slowest partition sets the pace, so this ratio is close to a direct multiplier on wall-clock time. The report also prints the smallest, median and largest reach sizes, which explain where any imbalance is coming from: if the largest reach on its own exceeds an even share, no assignment of whole reaches can balance the run, and the only remedies are fewer processors or a different stream network delineation. When no single reach exceeds an even share, as noted in the last lines above, the residual imbalance is the cost of keeping communication low, not a hard limit.

**Channel crossings** and **subsurface crossings** count the reach links that became MPI exchanges. Each one is communication performed every timestep, so both should be as low as the balance constraint allows. Remember that the channel crossings are counted within the subsurface crossings, not in addition to them.

**Reaches, headwaters and nodes per partition** show how the work was distributed. A partition holding many more reaches than its neighbors is not itself a problem, reaches vary hugely in size, and the Node% column is what matters — but a partition with an unusual headwater count is what the SSFH constraint exists to prevent.

Reusing or Supplying a Partition
--------------------------------

When *GRAPHFILE* is blank, tRIBS writes the partition it generated to ``<OUTFILENAME>_<method>_<n>nodes.reach``, where *method* is ``SF``, ``SSF`` or ``SSFH`` and *n* is the number of processors. An *OUTFILENAME* of ``results/test/parallel/bigsp`` run on four cores with SSF produces ``results/test/parallel/bigsp_SSF_4nodes.reach``. The path is printed to the console. This means the decomposition actually used is always recorded on disk, whether or not you asked for it.

Setting *GRAPHFILE* to an existing file reads that partition instead of generating one. This is worth doing to reproduce an earlier run exactly, to archive a decomposition alongside published results, or to supply a hand-built assignment. It is not needed for performance as generating a partition costs a negligible fraction of total simulation time. Nonetheless, the recommendation is to use the reuse the *GRAPHFILE* as the partitioning can differ between operating systems. Users typically copy the generated graph files into the location where the model input files are stored, rather than keeping in the output location.

A supplied file is fully validated against the current run. Every reach must be assigned exactly once, reach IDs must fall within the mesh's reach count, and partition IDs must fall within the current processor count. Each mismatch is a fatal error with an explanatory message, so a file generated for a different processor count or a different mesh is rejected rather than producing a subtly wrong decomposition.

Mapping a Partition in GIS
--------------------------

The time-integrated spatial output file (``*timestamp_00i``) records the rank of the processor that owned each node in a ``Proc`` column, described in :doc:`Output`. Because that column travels with the node IDs, joining it to the Voronoi mesh renders the decomposition directly as a map. Most commonly done using the Results class in the pytRIBS python package. This is a direct way to see whether a partition is spatially sensible whether partitions are compact, whether one has been split across the basin, whether the divisions follow the drainage structure.

Choosing a Number of Processors
-------------------------------

Improvements in computational time for supplying more processors diminish for the usual reasons: as partitions get smaller, each processor does less work per timestep while the number of boundary exchanges grows, and the load balance ratio typically worsens because whole reaches become coarse units relative to an even share.

Beyond that, because a reach is indivisible, the reach network itself imposes two hard limits on how far a given basin can usefully be parallelized. Both can be read off the partition statistics before committing to a run.

The absolute cap is the reach count. A domain of 63 reaches cannot be divided into more than 63 groups, so requesting more processors than the basin has reaches is a fatal error: tRIBS reports the mismatch and exits rather than starting a run with idle ranks.

The practical limit arrives well before that cap, and is set by the largest reach. That reach must land on one processor in one piece, so its node count is a floor on the size of the biggest partition, and therefore on the load balance ratio, which can never fall below:

    ``largest reach node count / (total nodes / processors)``

Big Spring's largest reach holds 307 of its 3748 nodes, so an even share stops being achievable once ``3748 / n`` drops below 307, at roughly twelve processors. Past that point the floor rises in proportion to the processor count: at 24 processors the best attainable balance is about 2.0, meaning one processor carries twice its share while the rest idle waiting on it. At some point, 36 processors for the example above, adding more processors will not improve the balance and will result in processors with nothing assigned to them. In that case tRIBS will exit the simulation with a warning. That does not mean 36 processors is the optimial choice for the example because with that setup there are exponentially more cross-processor communications required.