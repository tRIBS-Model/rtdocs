Model Input Formats
========================

The tRIBS model is designed to accept input from various types of data formats: grid data, TIN data, point data and text tables. The grid data can be time-invariant (soils and vegetation) or time-varying (rainfall and weather) grids. TIN mrsh data are inputted using two of methods depending on the application. The point data represent the values of time-varying forcings, such as meteorological data, that are available at specified points within the watershed. Resampling routines are available for geographically overlaying the grid or point data onto the Voronoi polygon mesh. Finally, text tables are used in the model for inputting parameter values associated with soil and vegetation maps or time series of hydrometeorological data.

Grid Input
---------------

A standard ASCII grid format is the default for all raster input, which may include soil and vegetation index maps, initial groundwater table depth, depth to bedrock, and hydrometeorological grids. Recent improvements allow the model to be compilied with GDAL, a popular open-source geospatial software package, this allows any raster type to be used in place of ASCII grid format. See the :doc:`QuickStart` page for details on how to compile the model with GDAL.

The ASCII grid format consists of a small, 6-line header that describes the matrix data presented in the text file, as shown in **Table 2.1**. This format is a convenient method for data exchange and is often used in Geographical Information Systems (GIS). Any extension name for the grid data can be used within tRIBS as long the filename is specified. Although each grid input is treated differently within the model, the grid input format should be identical. The grid pathname and file name are specified within the Input File. The model assumes that a coordinate systems with a spacing in meters along x, y, and z are used, such as the UTM coordin®ate system.

        **Table 2.1.** Example of a standard ASCII grid file.

        .. tabularcolumns:: |c|c|c|c|c|c|

        +-----------------+-----------+-----------+-----------+----------+----------+
        | ncols           | 6         |           |           |          |          |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | nrows           | 6         |           |           |          |          |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | xllcorner       | 346035    |           |           |          |          |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | yllcorner       | 3979905   |           |           |          |          |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | cellsize        | 2000      |           |           |          |          |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | NODATA_value    | -9999     |           |           |          |          |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | -9999           | -9999     | -9999     | 0.511     | 0.111    | -9999    |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | -9999           | -9999     | 0.951     | 1.873     | 0.239    | -9999    |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | -9999           | -9999     | 0.824     | 0.412     | 0.444    | 1.051    |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | -9999           | 3.123     | 0.154     | 0.853     | -9999    | -9999    |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | 1.090           | 2.541     | 0.288     | -9999     | -9999    | -9999    |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | 2.241           | 0.312     | -9999     | -9999     | -9999    | -9999    |
        +-----------------+-----------+-----------+-----------+----------+----------+

    **Table 2.1** has various features to note:

        1) the grid input must be composed of square grid cells (*i.e. dx = dy = cellsize* (in meters)),
        2) the coordinate system is specified by the values of x and y at the lower left corner (in this case, UTM zone 15 coordinates are shown),
        3) the entire grid can be rectangular, such that *nrows* and *ncols* can differ, and
        4) the NODATA_value can be used to represent cells outside of the domain of interest.

Mesh Input
--------------

Topographic data is inputted into the tRIBS Model through a two methods that are implemented in the tMesh class. The two available approaches are selected via the *OPTMESHINPUT* keyword (see :doc:`Model Input File`): providing a Points File that the model triangulates internally, or providing an already-constructed mesh as a set of four files.

The Points File (``*.points``) is the simplest and quickest mesh input to prepare, but offers the least control over the resulting mesh; previously this was the dominant method before the pytRIBS mesh-generation workflow, described below. An example is shown in **Table 2.2**. A Points File is a simple text file that contains a listing of point coordinates (*x, y, z*) and the boundary code (*b*) for each point (in that specific order) with a small header that indicates the total number of points in the file. The boundary code is indicative of the node position within the watershed: 0 (mesh interior node), 1 (closed mesh boundary node), 2 (open mesh boundary or outlet node), 3 (stream network node). Proper creation and consistency of the Points File is very important for the tRIBS Model and the ``*.points`` file should be carefully inspected.

        **Table 2.2.** Example of a tRIBS Points File (``*.points``)

        .. tabularcolumns:: |c|c|c|c|

        +---------+----------+----------+----------+
        | 7       |          |          |          |
        +---------+----------+----------+----------+
        | 405     | 0        | 0        | 1        |
        +---------+----------+----------+----------+
        | 495     | 0        | 0        | 2        |
        +---------+----------+----------+----------+
        | 585     | 0        | 0        | 1        |
        +---------+----------+----------+----------+
        | 360     | 90       | 1        | 1        |
        +---------+----------+----------+----------+
        | 450     | 90       | 1        | 0        |
        +---------+----------+----------+----------+
        | 540     | 90       | 1        | 0        |
        +---------+----------+----------+----------+
        | 630     | 90       | 1        | 1        |
        +---------+----------+----------+----------+

Alternatively, tRIBS can read in an already-constructed mesh directly, as the set of four files: ``*.nodes``, ``*.edges``, ``*.tri`` and ``*.z``. These describe the TIN mesh properties in full detail, including the connectivity between nodes and the triangles within the mesh, and are the same files tRIBS itself writes at the end of a run (allowing a mesh to be reused across subsequent runs without repeating the costly triangulation).

These four mesh files can also be generated directly, without ever creating a Points File, using the mesh-generation workflow in the `pytRIBS <https://github.com/tRIBS-Model/pytRIBS>`_ package. This workflow gives substantially more control over how the mesh is constructed than the Points File approach (for example, refinement and boundary handling), and can also generate a ``*.points`` file if one is wanted for use with the legacy triangulation path. An example of this workflow is available in `this repository <https://github.com/tRIBS-Model/tRIBS-Workshop-Sandbox>`_.

Point Station Input
-------------------------

Hydrometeorological data can be inputted into the tRIBS model through methods for Point Station Input implemented in the ``tEvapoTrans`` and ``tRainfall`` classes and the ``tHydroMet`` and ``tRainGauge`` storage classes. Point Station Input is useful for providing meteorological data from weather stations or rain gauges. The data from these sparse stations or points is resampled by using a Thiessen polygon method at the point coordinates. The station properties, including coordinates, are specified through an SDF file (Station Descriptor File), while the station data are provided in an MDF file (Meteorological Data File).

Text File Inputs
----------------------

Various types of text files are used in the tRIBS Model to specify model options, hydrologic parameters or control commands. The most important of the text files is the Model Input File (``*.in``). This file contains various required and optional parameters organized by keywords. The format for each parameter consists of a line of descriptive text followed by the value of the parameter itself on a second line. There are over 100 different keyword inputs in a typical Model Input File. These can be classified into various groupings: Model Run Parameters, Model Run Options and Model Input Files and Pathnames. Subgroupings include: Time Variables, Routing Variables, Mesh Generation, Resampling Grids, Meteorological Data and Output Data. More details concerning the Model Input File will be presented in the section on Model Input File in this document. An example ``.in`` file is provided on the :doc:`Templates` page.

Another important use of text files is for the reclassification of soil and land use grids into meaningful hydrologic parameters assigned to each Voronoi polygon. A simple CSV file is used to relate each cover class to the particular hydrologic parameter required for the model equations. In the case of the soil reclassification table (``*.sdt``), the parameters are used to specify the soil hydraulic and thermal properties. In the case of the land reclassification table (``*.ldt``), the parameters are used to relate the cover type to the interception and evapotranspiration properties of the vegetation and land cover. See :doc:`Model_Parameters_Forcings` for the detailed file structures of these and the other CSV-based parameter and forcing files (station descriptor/data files, grid data files).

A shell script can also be used to run the model and specify the command line options desired during the run by using a Model Run File (``*_run``). This file consists of a single line that specifies the pathname of the tRIBS executable followed by the name of the Model Input File and the desired command line options. For examples see the :doc:`Templates` page.

Parallel Model Inputs
-----------------------------------

The parallel mode is selected with the keyword *PARALLELMODE* in the tRIBS Input file (``*.in``). This section describes the reach partition file (``*.reach``), which records how a watershed domain is divided into groups of stream reaches and which computer processor each group is run on.

As of v6.0.0 this file is **not a required input**. tRIBS builds the reach graph from the mesh at startup and partitions it internally, writing the result to this format. The file remains relevant in two situations: tRIBS writes one on every parallel run when the *GRAPHFILE* keyword is blank, so the decomposition actually used is always recorded on disk; and an existing file can be supplied through *GRAPHFILE* to reproduce a previous decomposition or impose a hand-built one. See :doc:`Parallel_Simulations` for more information.

          **Table 2.3** Reach Partition File (``*.reach``)

          .. tabularcolumns:: |c|c|

          +-------------------------+-------------------------+
          | Processor ID (#)        | Reach ID (#)            |
          +-------------------------+-------------------------+
          | Processor ID (#)        | Reach ID (#)            |
          +-------------------------+-------------------------+
          | Processor ID (#)        | Reach ID (#)            |
          +-------------------------+-------------------------+
          | Processor ID (#)        | Reach ID (#)            |
          +-------------------------+-------------------------+
          | ...                     | ...                     |
          +-------------------------+-------------------------+

The reach partition file (**Table 2.3**) is a two-column text file with no header, written in the same format tRIBS has always read. Column 1 holds the numerical ID of the computer processor (labeled from 0 to N-1, where N is the number of MPI processes) and Column 2 holds the numerical ID of a reach (labeled from 0 to M) assigned to that processor. Every reach in the mesh must appear exactly once. Reach IDs correspond to those in the ``*__reach`` file generated by the tRIBS model after mesh construction.

Because the file is plain text, a generated partition can be inspected, version-controlled, or edited by hand. Building one from scratch is rarely worthwhile: load balancing across sub-basins is exactly what the internal partitioner optimizes, and partition-only mode reports the resulting balance and edge cuts directly.


.. _MeshBuilder:

MeshBuilder
~~~~~~~~~~~~

MeshBuilder is a standalone utility that generates ``*.reach`` graphfiles by exporting reach connectivity and driving ``gpmetis`` through a set of Perl scripts. **As of v6.0.0 it is no longer needed or recommended for parallel tRIBS simulations**, since tRIBS performs the equivalent partitioning internally and requires no external tools.

Reservoir Model Input
-----------------------------------

The input of reservoir data into tRIBS enables the level pool routing simulation within the hydraulic channel routing scheme. To enable this routing option, there are two main files the user is required to provide. The Reservoir Polygon ID File provides information concerning the selected nodes to be used as Reservoirs. **Table 2.4** presents the format required in the Polygon ID file (``*.res``). The number of reservoirs (*nReservoirs*) specifies the number of TIN nodes (Voronoi polygons) that will be used as dam locations in the simulation. *nNodeParams* are the number of parameters required for each node, which should always be set at 3. In the body of the file, the user should include the ID number of the TIN node in the first column (*NodeID*, int, node selected by the user as a reservoir), followed by the type of reservoir the node will be (*ResNodeType*, int, type of reservoir associated with the node, linked to the *RESDATA* information) and the initial water surface elevation (*Initial_H*, double, meters) at the reservoir in the third column (empty reservoir should be specified as 0.0 m). When assigning the node to be used as a reservoir, the user should assign nodes that correspond to the start or the end of a river reach, to do so it is recommended to use the Voronoi mesh and stream network to identify potential nodes. An example of a ``*.res`` file is presented in **Table 2.5**.

    **Table 2.4** Format for the Reservoir Polygon ID File (``*.res``).

            .. tabularcolumns::  |c|c|c|

            +----------------+-----------------+-----------------+
            | *#Reservoirs*  |  *nNodeParams*  |                 |
            +----------------+-----------------+-----------------+
            | *NodeID*       |   *ResNodeType* |  *Initial_H*    |
            +----------------+-----------------+-----------------+

    **Table 2.5** Example of a Reservoir Polygon ID File (``*.res``).

            .. tabularcolumns::  |c|c|c|

            +-----------+---------+-------+
            |  *4*      |   *3*   |       |
            |           |         |       |
            +-----------+---------+-------+
            |  *578867* | *0*     | *0.0* |
            +-----------+---------+-------+
            |  *575490* | *1*     | *0.0* |
            +-----------+---------+-------+
            |  *573514* | *2*     | *0.0* |
            +-----------+---------+-------+
            |  *574354* | *3*     | *0.0* |
            +-----------+---------+-------+

The second file that the user should provide is the *RESDATA* information (``*.eds``) related to the elevation-discharge-storage data of each reservoir type. **Table 2.6** presents the format required for the elevation-discharge-storage data file (``*.eds``). The header will include the number of types (*nTypes*) of reservoirs and the number of reservoir parameters (*nResParams*) required which should always be set to 4. The identifier for the type of reservoir should start with the number zero and be repeated for each row that describes an individual reservoir type. For a second reservoir type, the identifier would have a number of one. The second column will have the elevation (in meters) with the corresponding discharge (*m3/s*, double) and storage (1000 m3, double) for that elevation in the third and fourth column. An example of the reservoir data file (*RESDATA*) is presented in **Table 2.7**, notice the change from 0 to 1 in the first column indicating the change from one reservoir type to another.

    **Table 2.6** Format for the Reservoir Data File (``*.eds``).

            .. tabularcolumns::  |c|c|c|c|

            +-----------+-------------------+----------------------------------------------+
            | *nTypes*  |  *nResParams*     |                                              |
            +-----------+-------------------+----------------------------------------------+
            | *Type#*   |  *Elevation (m)*  |  *Discharge (m3/s)*   |  *Storage (1000m3)*  |
            +-----------+-------------------+----------------------------------------------+

    **Table 2.7** Example of a Reservoir Data File (``*.eds``).

            .. tabularcolumns::  |c|c|c|c|

            +------+-------+--------+--------+
            | *2*  |  *4*  |                 |
            |      |       |                 |
            +------+-------+--------+--------+
            | *0*  |  *0*  |  *0*   |  *0*   |
            +------+-------+--------+--------+
            | *0*  | *0.5* | *50*   |  *10*  |
            +------+-------+--------+--------+
            | *0*  |  *1*  | *350*  |  *50*  |
            +------+-------+--------+--------+
            | *0*  | *1.5* | *1200* |  *300* |
            +------+-------+--------+--------+
            | *0*  |  *2*  | *1500* |*12000* |
            +------+-------+--------+--------+
            | *1*  |  *0*  | *0*    |  *0*   |
            +------+-------+--------+--------+
            | *1*  | *0.5* | *10*   |  *100* |
            +------+-------+--------+--------+
            | *1*  |  *1*  | *20*   |  *400* |
            +------+-------+--------+--------+
            | *1*  | *1.5* | *30*   |  *800* |
            +------+-------+--------+--------+
            | *1*  |  *2*  | *40*   | *1600* |
            +------+-------+--------+--------+


