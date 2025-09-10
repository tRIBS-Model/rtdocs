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
Note: This step can be skipped if you prefer and are able to use the provided :doc:`Executables`. If so the executable can be unpacked by running ``path/to/tRIBS-5.2.0-*.sh`` in the command line.

1. Download tRIBS source code from the main branch of the GitHub repository. A direct download link is available `here <https://github.com/tribshms/tRIBS/archive/refs/heads/main.zip>`_. Unzip the repository and using the command line to change to the repository directory. Once in this directory you should see the ``CMakeLists.txt`` file and the ``src`` sub-directory which contains the tRIBS source code. The code block below provides an example of how to do this but you will need to update the path realative to where you have downloaded the tRIBS-main repository.

	.. code-block:: bash
		
		cd path/to/tRIBS-main && ls

2. Run CMake to build and link the source code. From the main tRIBS directory you can run the following commmands: 

	.. code-block:: bash

		cmake -S . -B build -Dparallel=ON -DCMAKE_CXX_FLAGS="-O2"
		cmake --build build --target all

  The first argument of the first line of code specifies where the required source files (i.e. ``CMakeLists.txt`` and ``src``) are located (i.e. ``-S .``). The second argument specfies where the executable will be built (i.e. ``-B build``). If the directory ``build`` doesn't exist CMake will generate it for you. You can also provide additional compilation flags: Here we provide the flags ``-Dparallel=ON`` which will generate the parallel executable ``tRIBSpar`` and ``-DCMAKE_CXX_FLAGS="-O2"`` which optimizes the executable for additional computational speed.  The second line builds the executable from the newly configured files in the build directory and generates the executable.

Setup Benchmark
---------------

1. Download the Big Spring benchmark `here <https://doi.org/10.5281/zenodo.17088972>`_.

2. Copy ``tRIBSpar`` into the Big Spring bin sub-directory. Note you will have to double-check that your paths are correct for both locations. Below is an example that will need to be modified.

   .. code-block:: bash

    	cp tRIBS-main/build/tRIBSpar big_spring/bin/tRIBSpar


Run tRIBS Simulation
--------------------

1. To run the tRIBS simulation you will need to be located in the root directory of the Big Spring bench mark. Using the command line you can change into the directory as follows--but as noted above the exact path will depend on where you have downloaded or moved the Big Spring Benchmark.

	.. code-block:: bash

		cd ./big_spring/

2. Once you are located in the root directory, you can then run the tRIBS executable. It is important to note that you will have to provide an input file as an argument and if running in parallel you will also need to use ``mpirun`` and specify the number of processors to use via the ``-n`` flag. For the Big Spring Benchmark the input file has already been constructed with paths relative from the root directory (hence the need to run executable from there). The input file is located at ``src/in_files/big_spring_par.in``. There is also a serial input file ``big_spring.in`` in the same directory and if you built the tRIBS executable by passing the compiler flag ``-Dparallel=OFF`` you would provide this file as the input instead. Below we provide an example of how to execute tRIBS using three processors. Note its possible to use more processors, however, tRIBS may not always take advantage of the additional resources if the mesh has not been partitioned to account for additional processors.

	.. code-block:: bash
		
		mpirun -n 3 bin/tRIBSpar src/in_files/big_spring_par.in

 If the model executes correctly you will see the model printing output to the command line as it steps through model initation and the simulation loop. Note the simulation, for a reasonably fast computer, could still take 10 to 20 minutes to complete. Additionally, some users may find it useful to redirect the model output from the command line to a text file using ``> output.txt``.

Viewing tRIBS Results
---------------------

Once the model has successfully terminated, you should be able to see that the ``results\test\parallel`` sub-directory has been populated with a number of different files. See the `documentation <https://tribshms.readthedocs.io/en/latest/man/Output.html#model-outputs>`_ for details on what the individual files entail. Here we are going to focus on creating a spatial map of mean evapotranspiration rates over the length of the simulation. To do this we will use Python, where we have provided the module ``read_voi.py`` under the directory ``doc/notebooks``. Note, the `pytRIBS package <https://pypi.org/project/pytRIBS/>`_ may also be used for similar purposes but it is currently under development. For the following section we assume python 3 has been installed on you machine.

1. We recommended you use a virtual environment for this exercise and we provide an example of how to do this below as well as steps required for installing required packages. The code below assumes that you are located in the Big Spring root directory.

	.. code-block:: bash

		python3 -m venv big_spring_env #can be created directly in the big_spring directory
		source big_spring_env/bin/activate # activate the virtual environment
		pip install -r doc/notebooks/requirements.txt # install related packages
		pip install jupyterlab # this is optional but can be used to view doc/notebooks/Results.ipynb

2. Once you have successfully installed the required packages you then can use python to visualize the results. Below we provide an example code snippet that can be used to plot a Voronoi Diagram with each cell colored by the average evapotranspiration rate over the entire simulation. Note here we use the variable ``'AvET'`` but users can update this code with any of the variables found `here <file:///Users/wr/Documents/Repos/Forked/rtdocs/docs/_build/html/man/Output.html#time-integrated-spatial-output-table>`_

	.. code-block:: python

		import geopandas as gpd
		import pandas as pd
		import matplotlib.pyplot as plt
		import matplotlib.font_manager as fm
		import matplotlib as mpl
		import numpy as np
		from matplotlib_scalebar.scalebar import ScaleBar

		# helper scripts to read in spatial results using pandas and geopandas
		from doc.notebooks import read_voi

		# merge results from different processors
		par_results = 'results/test/parallel/'
		int_df_par = read_voi.merge_parallel_spatial_files(f'{par_results}bigsp',35072)

		# generate the voronoi diagaram with variable from integrated file
		voi_par = read_voi.merge_parallel_voi(f'{par_results}bigsp_voi',join=int_df_par['35072'])

		# see the documentation to see possible variables for plotting
		fig,ax = plt.subplots()
		low = np.percentile(voi_par['AvET'], 2.5)
		high = np.percentile(voi_par['AvET'], 97.5)
		voi_par.plot(ax=ax,
					column='AvET', 
					cmap='YlOrBr',
					legend=True,
					vmin=low,
					vmax=high,
					legend_kwds={'label': r'ET in mm/hr','orientation': 'horizontal',"shrink":.5})
		ax.add_artist(ScaleBar(1,location='lower left'))
		plt.title('Parallel, Big Spring, Arizona, USA: Map of Mean Evapotranspiration Rate')
		plt.axis('off')
		plt.show()

  The resulting plot should appear as provided below: 

  .. image:: ../images/QuickStart_example.png
    :alt: Alternative text for the image
    :width: 680px
    :align: center




