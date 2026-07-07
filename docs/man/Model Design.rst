Model Design
=================

    tRIBS is written in C++ and organized around a straightforward data flow through the simulation. A triangulated mesh first discretizes the watershed into Voronoi polygons (``tMesh``, ``tMeshElements``); time-varying meteorological and precipitation forcing is then read and resampled onto that mesh (``tRasTin``, ``tHydro``); at each node, the hydrologic process models compute infiltration, evapotranspiration, and snow processes (``tHydro``); the resulting fluxes are routed downslope and through the channel network (``tFlowNet``); and the accumulated state and fluxes are written out at each requested interval (``tInOut``). The ``tSimulator`` classes drive this sequence forward in time, calling into each stage at every timestep. In order to best describe the software architecture of the model, it is important to first understand the file structure. **Tables 1.1** and **1.2** list the directories and files that form part of tRIBS. For the new user, this is a starting point to begin to form a mental picture of how the model operates.

Model File Structure
--------------------------

    The tRIBS Model is organized into a single directory (called ``tRIBS``) with various sub directories that contain the model C++ classes. Each sub directory encapsulates classes with similar functionality or behavior. **Table 1.1** shows the sub directories as a user would see upon downloading the source code. Various of these sub directories deal with the hydrologic processes (``tHydro``, ``tFlowNet``, ``tRasTin``), others create the mesh architecture (``tMesh``, ``tMeshElements``, ``tMeshList``), while others are general purpose classes used for model execution (``tSimulator``, ``tInOut``, ``tCNode``) or within other classes (``tArray``, ``tList``, ``tPtrList``).  The ``Headers`` and ``Mathutil`` directories contain global header files and mathematical utilities for the model, respectively. Two subdirectories have been added for parallelization (``tGraph``, ``tParallel``).

        **Table 1.1** tRIBS Model Subdirectories

        .. tabularcolumns:: |c|c|c|

        +--------------------+--------------------+--------------------+
        |  Headers           |  tInOut            |  tMeshList         |
        +--------------------+--------------------+--------------------+
        |  Mathutil          |  tList             |  tPtrList          |
        +--------------------+--------------------+--------------------+
        |  utilities         |  tListInputData    |  tRasTin           |
        +--------------------+--------------------+--------------------+
        |  tArray            |  tMesh             |  tSimulator        |
        +--------------------+--------------------+--------------------+
        |  tCNode            |  tMeshElements     |  tGraph            |
        +--------------------+--------------------+--------------------+
        |  tHydro            |  tFlowNet          |  tParallel         |
        +--------------------+--------------------+--------------------+

    In addition to the sub directories, the ``tRIBS`` directory contains the main function (``main.cpp``) and the CMake build configuration (``CMakeLists.txt``). tRIBS uses CMake for an out-of-source build: from a build directory, ``cmake -Dparallel=OFF ..`` followed by ``make`` compiles the serial executable (``tRIBS``), while ``cmake -Dparallel=ON ..`` produces the MPI parallel executable (``tRIBSpar``). GDAL support for raster I/O can be enabled with ``-DWITH_GDAL=ON``. Each sub directory of the source code includes the C++ class files (``*.cpp`` used as convention) and the C++ Header Files (``*.h``). **Table 1.2** shows a list of the code files in the tRIBS model for further reference.

        **Table 1.2** tRIBS Model Class and Header Files

        .. tabularcolumns:: |c|l|

        +--------------------+-------------------------------------------------------------------+
        |  tRIBS             |  main.cpp, CMakeLists.txt                                         |
        +--------------------+-------------------------------------------------------------------+
        |  /Headers          |  Classes.h, Definitions.h, Inclusions.h, globalFns.h,             |
        +--------------------+-------------------------------------------------------------------+
        |                    |  globalFns.cpp, TemplDefinitions.h, globalIO.h                    |
        +--------------------+-------------------------------------------------------------------+
        |  /Mathutil         |  geometry.h , mathutil.h, mathutil.cpp,                           |
        +--------------------+-------------------------------------------------------------------+
        |                    |  predicates.h, predicates.cpp                                     |
        +--------------------+-------------------------------------------------------------------+
        |  /utilities        |  InitialGW.cpp, RunsTracker.cpp, RainInputCheck.cpp,              |
        +--------------------+-------------------------------------------------------------------+
        |                    |  mergeOutput.pl                                                   |
        +--------------------+-------------------------------------------------------------------+
        |  /tArray           |  tArray.h, tMatrix.h, tMatrix.cpp                                 |
        +--------------------+-------------------------------------------------------------------+
        |  /tCNode           |  tCNode.h, tCNode.cpp                                             |
        +--------------------+-------------------------------------------------------------------+
        |  /tFlowNet         |  tFlowNet.h, tFlowNet.cpp, tFlowResults.h, tFlowResults.cpp,      |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tKinemat.h, tKinemat.cpp, tReservoir.cpp, tReservoir.h,          |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tResData.cpp, tResData.h                                         |
        +--------------------+-------------------------------------------------------------------+
        |  /tGraph           |  tGraph.h, tGraph.cpp, tGraphNode.h, tGraphNode.cpp               |
        +--------------------+-------------------------------------------------------------------+
        |  /tHydro           |  tEvapoTrans.h, tEvapoTrans.cpp, tHydroMet.h, tHydroMet.cpp,      |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tHydroModel.h, tHydroModel.cpp, tIntercept.h, tIntercept.cpp,    |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tWaterBalance.h, tWaterBalance.cpp, tSnowPack.h, tSnowPack.cpp   |
        +--------------------+-------------------------------------------------------------------+
        |  /tInOut           |  tInputFile.h, tInputFile.cpp, tOutput.h, tOutput.cpp,            |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tOstream.h, tOstream.cpp                                         |
        +--------------------+-------------------------------------------------------------------+
        |  /tList            |  tList.h, tList.cpp                                               |
        +--------------------+-------------------------------------------------------------------+
        |  /tListInputData   |  tListInputData.h, tListInputData.cpp                             |
        +--------------------+-------------------------------------------------------------------+
        |  /tMesh            |  tMesh.h, tMesh.cpp, tTriangulator.h, tTriangulator.cpp,          |
        +--------------------+-------------------------------------------------------------------+
        |                    |  heapsort.h                                                       |
        +--------------------+-------------------------------------------------------------------+
        |  /tMeshElements    |  meshElements.h, meshElements.cpp                                 |
        +--------------------+-------------------------------------------------------------------+
        |  /tMeshList        |  tMeshList.h                                                      |
        +--------------------+-------------------------------------------------------------------+
        |  /tParallel        |  tTimer.h, tTimer.cpp, tTimings.h, tTimings.cpp,                  |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tParallel.h, tParallel.cpp                                       |
        +--------------------+-------------------------------------------------------------------+
        |  /tPtrList         |  tPtrList.h, tPtrList.cpp                                         |
        +--------------------+-------------------------------------------------------------------+
        |  /tRasTin          |  tInvariant.h, tInvariant.cpp, tRainfall.h, tRainfall.cpp,        |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tResample.h, tResample.cpp, tVariant.h, tVariant.cpp,            |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tRainGauge.h, tRainGauge.cpp,                                    |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tShelter.h, tShelter.cpp                                         |
        +--------------------+-------------------------------------------------------------------+
        |  /tSimulator       |  tRunTimer.h, tRunTimer.cpp,  tRestart.h, tRestart.cpp,           |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tSimul.h, tSimul.cpp, tControl.h, tControl.cpp,                  |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tPreProcess.h, tPreProcess.cpp,                                  |
        +--------------------+-------------------------------------------------------------------+

    The class names are indicative of the functionality for that particular class. Most files contain a single class that encapsulate the data and functions operating on the data within a single object. In some occasions, it has been convenient to include several interrelated classes within the same file. A list of all non-derived tRIBS Classes can be found in ``tRIBS/Headers/Classes.h``. ``main.cpp`` is used in tRIBS to construct the various objects, while the simulation control is performed by ``tSimul.cpp``. 

Computational Mesh
------------------------

    The tRIBS Model inherited the Triangulated Irregular Network (TIN) mesh architecture from the CHILD model (Tucker *et al.*, 1999) using various options in the ``tMesh`` class. In addition, new input capabilities take advantage of the TIN creation capabilities in external multiple reslution mesh generators to represent real world watersheds as "hydrologically" significant TINs. The most used options for creating the computational mesh are the following:

      - Generate a new mesh from a given set of coordinates (x , y , z, b) with a boundary flag (``*.points``).
      - Generate a new mesh using the outputs of tRIBS Meshbuilder for large domains (``*.nodes``, ``*.edges``, ``*.tri``, ``*.z``).
      - Read in existing tRIBS Mesh files from a previous run (``*.nodes``, ``*.edges``, ``*.tri``, ``*.z``).

    A TIN within these methods is a set of highly interconnected triangle objects with three edge and three node objects (as defined in ``meshElements.cpp``). The TIN mesh allows for flow from TIN node to TIN node, along a triangle edge, using a finite difference approach. Hydrologic computations made at each TIN node (e.g. infiltration, evaporation, groundwater table elevation) are assumed valid over a region consisting of the Voronoi polygon associated with the node. In this way the Voronoi polygon is used as the control volume for mass conservation. The Voronoi polygon is the dual diagram of the TIN mesh and can be computed by the intersection of perpendicular bisectors to each TIN edge.

