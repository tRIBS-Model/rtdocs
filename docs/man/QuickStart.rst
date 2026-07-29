Quick Start
===========

This guide provides a walk-through for building and running tRIBS on the Big Spring Benchmark (see :doc:`Benchmarks`). To successfully navigate this guide you will need basic knowledge of using the unix shell and understanding of computer file structures. If you are unfamiliar with basic unix shell commands Software Carpentry provides a nice tutorial `here <https://swcarpentry.github.io/shell-novice/>`_.

Install Requirements
--------------------

Building tRIBS requires CMake and if you are running tRIBS in parallel you will also need access to an MPI library, here we use `OpenMPI <https://open-mpi.org/>`_ since tRIBS has been tested and developed with this library.

1. Install CMake 

   For MacOS use:

   .. code-block:: bash

      brew install cmake

   For Linux (Ubuntu) use:

   .. code-block:: bash

      sudo apt-get -y install cmake

2. Install OpenMPI

   For MacOS use:

   .. code-block:: bash

      brew install openmpi

   For Linux (Ubuntu) use:

   .. code-block:: bash

      sudo apt-get install openmpi-bin openmpi-doc libopenmpi-dev  

Build tRIBS executable
----------------------
Note: This step can be skipped if you prefer and are able to use the provided :doc:`Executables`. If so the executable can be unpacked by running ``path/to/tRIBS-6.0.0-*.sh`` in the command line.

1. Download tRIBS source code from the main branch of the GitHub repository. A direct download link is available `here <https://github.com/tRIBS-Model/tRIBS/archive/refs/heads/main.zip>`_. Unzip the repository and using the command line to change to the repository directory. Once in this directory you should see the ``CMakeLists.txt`` file and the ``src`` sub-directory which contains the tRIBS source code. The code block below provides an example of how to do this but you will need to update the path realative to where you have downloaded the tRIBS-main repository.

	.. code-block:: bash
		
		cd path/to/tRIBS-main && ls

2. Run CMake to build and link the source code. From the main tRIBS directory you can run the following commmands: 

	.. code-block:: bash

		cmake -S . -B build -Dparallel=ON -DCMAKE_CXX_FLAGS="-O2"
		cmake --build build --target all

  The first argument of the first line of code specifies where the required source files (i.e. ``CMakeLists.txt`` and ``src``) are located (i.e. ``-S .``). The second argument specfies where the executable will be built (i.e. ``-B build``). If the directory ``build`` doesn't exist CMake will generate it for you. You can also provide additional compilation flags: Here we provide the flags ``-Dparallel=ON`` which will generate the parallel executable ``tRIBSpar`` and ``-DCMAKE_CXX_FLAGS="-O2"`` which optimizes the executable for additional computational speed.  The second line builds the executable from the newly configured files in the build directory and generates the executable.

Setup Benchmark
---------------

1. Download the Big Spring benchmark `here <https://github.com/tRIBS-Model/tRIBS-benchmarks>`_.

2. Copy ``tRIBSpar`` into the Big Spring bin sub-directory. Note you will have to double-check that your paths are correct for both locations. Below is an example that will need to be modified.

   .. code-block:: bash

    	cp tRIBS-main/build/tRIBSpar big_spring/bin/tRIBSpar


Run tRIBS Simulation
--------------------

1. To run the tRIBS simulation you will need to be located in the root directory of the Big Spring bench mark. Using the command line you can change into the directory as follows--but as noted above the exact path will depend on where you have downloaded or moved the Big Spring Benchmark.

	.. code-block:: bash

		cd ./big_spring/

2. Once you are located in the root directory, you can then run the tRIBS executable. It is important to note that you will have to provide an input file as an argument and if running in parallel you will also need to use ``mpirun`` and specify the number of processors to use via the ``-n`` flag. For the Big Spring Benchmark the input file has already been constructed with paths relative from the root directory (hence the need to run executable from there). The input file is located at ``src/in_files/big_spring_par.in``. There is also a serial input file ``big_spring.in`` in the same directory and if you built the tRIBS executable by passing the compiler flag ``-Dparallel=OFF`` you would provide this file as the input instead. Below we provide an example of how to execute tRIBS using three processors. The domain is partitioned automatically for whatever number of processors you request, so you can change ``-n`` freely without preparing any additional files; returns do diminish once partitions become small relative to the communication between them. See :doc:`Parallel_Simulations` for how to check whether a given processor count partitions the basin well.

	.. code-block:: bash
		
		mpirun -n 3 bin/tRIBSpar src/in_files/big_spring_par.in

 If the model executes correctly you will see the model printing output to the command line as it steps through model initation and the simulation loop. Note the simulation, for a reasonably fast computer, could still take 10 to 20 minutes to complete. Additionally, some users may find it useful to redirect the model output from the command line to a text file using ``> output.txt``.

Viewing tRIBS Results
---------------------

Once the model has successfully terminated, you should be able to see that the ``results/test/parallel`` sub-directory has been populated with a number of different files. See :doc:`Output` for details on what the individual files entail. Parallel spatial output is merged automatically by the model, so there is no separate per-processor merge step to run yourself.

The recommended way to load and explore these results is with the `pytRIBS package <https://github.com/tRIBS-Model/pytRIBS>`_, which reads tRIBS output directly and includes built-in plotting methods for common visualizations, such as mapping a spatial variable (e.g. average evapotranspiration) over the Voronoi mesh. For a complete, worked example using this Big Spring output, see the pytRIBS examples referenced from :doc:`Examples`.




