Templates and Examples
========================

The best way to learn the tRIBS model setup is to inspect a complete, working example. The `tRIBS Benchmarks repository`_ provides the most up-to-date and useful examples for running the model.

We recommend using the input files and run commands from the benchmarks as a starting point for your own simulations.

tRIBS Input File (*.in)
-------------------------

The main input file contains all the runtime options and paths to the required data files. The `big_spring.in` file from the watershed-scale benchmark is an excellent, well-documented example that uses a wide range of model features.

*   **View the template here:** `watershed-scale-big-spring/src/in_files/big_spring.in`_

Execution Scripts and Commands
------------------------------

Instead of providing static shell scripts, we provide the direct commands needed to run the model. These can be easily incorporated into your own shell scripts.

**Example for running tRIBS in serial mode:**

This command runs the standard tRIBS executable with a given input file.

.. code-block:: bash

   /path/to/your/tRIBS/executable src/in_files/big_spring.in

**Example for running tRIBS in parallel mode:**

This command uses `mpirun` to launch the parallel tRIBS executable (`partRIBS`) on 3 processor cores.

.. code-block:: bash

   mpirun -n 3 /path/to/your/partRIBS/executable src/in_files/big_spring_par.in

For full context on these commands and the required files, please see the README files within the `tRIBS Benchmarks repository`_.

.. _tRIBS Benchmarks repository: https://github.com/tRIBS-Model/tRIBS-benchmarks
.. _watershed-scale-big-spring/src/in_files/big_spring.in: https://github.com/tRIBS-Model/tRIBS-benchmarks/blob/main/watershed-scale-big-spring/src/in_files/big_spring.in