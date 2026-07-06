Model Input File
===============================

As with any model, half the battle in getting a correct model run is in providing the appropriate model input. Without having all the correct model input, the tRIBS Model will exit with an appropriate error message. 

Input File Contents
-------------------

The tRIBS Model Input File (``*.in``) is currently the primary user interface to the model. Although not a graphical medium, it is an easy and efficient means of manipulating all modeling options, parameters and inputs. Those parameters that are not required for a particular run are ignored by the model. The ``*.in`` file is organized by keywords. The format for each parameter consists of a line of descriptive text followed by the value of the parameter itself on a second line. **Table 4.1** presents a list of the keywords. Note that all keywords are capitalized. The values associated with each parameter may be a number or a string. If the units are specified as ints or doubles, this implies that the parameters are dimensionless, otherwise a unit is expressed. The **difference between a pathname and a base pathname** is simply that the pathname includes the entire path plus the entire name of the file, including the extension, while a base pathname is only the path and the base name of the file (no extension). See :doc:`Templates` for an example input file.

            **Table 4.1** List of Model Parameters in tRIBS Model Input File

            .. tabularcolumns:: |c|c|l|

            +-----------------------+-----------------+----------------------------------------------------+
            | Keyword               | Units           | Description                                        |
            +=======================+=================+====================================================+
            | *STARTDATE*           | *date format*   | Date of start of simulation                        |
            +-----------------------+-----------------+----------------------------------------------------+
            | *RUNTIME*             | *hours*         | Total number of hours in run                       |
            +-----------------------+-----------------+----------------------------------------------------+
            | *TIMESTEP*            | *mins*          | Unsaturated zone time step                         |
            +-----------------------+-----------------+----------------------------------------------------+
            | *GWSTEP*              | *mins*          | Saturated zone time step                           |
            +-----------------------+-----------------+----------------------------------------------------+
            | *METSTEP*             | *mins*          | Meteorological data input time step                |
            +-----------------------+-----------------+----------------------------------------------------+
            | *RAININTRVL*          | *hours*         | Rainfall data input time step                      |
            +-----------------------+-----------------+----------------------------------------------------+
            | *INTSTORMMAX*         | *hours*         | Interstorm interval                                |
            +-----------------------+-----------------+----------------------------------------------------+
            | *RAINSEARCH*          | *hours*         | Rainfall search interval                           |
            +-----------------------+-----------------+----------------------------------------------------+
            | *BASEFLOW*            | *m3/s*          | Minimum baseflow discharge                         |
            +-----------------------+-----------------+----------------------------------------------------+
            | *KINEMVELCOEF*        | *double*        | Kinematic routing velocity coefficient             |
            +-----------------------+-----------------+----------------------------------------------------+
            | *VELOCITYRATIO*       | *double*        | Stream-hillslope velocity coefficient              |
            +-----------------------+-----------------+----------------------------------------------------+
            | *FLOWEXP*             | *double*        | Nonlinear discharge coefficient                    |
            +-----------------------+-----------------+----------------------------------------------------+
            | *CHANNELROUGHNESS*    | *double*        | Uniform channel roughness value                    |
            +-----------------------+-----------------+----------------------------------------------------+
            | *CHANNELWIDTH*        | *double*        | Uniform channel width                              |
            +-----------------------+-----------------+----------------------------------------------------+
            | *CHANNELWIDTHCOEFF*   | *double*        | Coefficient in width-area relation                 |
            +-----------------------+-----------------+----------------------------------------------------+
            | *CHANNELWIDTHEXPNT*   | *double*        | Exponent in width-area relation                    |
            +-----------------------+-----------------+----------------------------------------------------+
            | *CHANNELWIDTHFILE*    | *pathname*      | Input file name for channel widths                 |
            +-----------------------+-----------------+----------------------------------------------------+
            | *CHANNELCONDUCTIVITY* | *mm/hr*         | Hydraulic conductivity in channel                  |
            +-----------------------+-----------------+----------------------------------------------------+
            |*TRANSIENTCONDUCTIVITY*| *mm/hr*         | Conductivity during transient period               |
            +-----------------------+-----------------+----------------------------------------------------+
            | *TRANSIENTTIME*       | *double*        | Time until transient period ends                   |
            +-----------------------+-----------------+----------------------------------------------------+
            | *CHANNELPOROSITY*     | *double*        | Porosity in channel                                |
            +-----------------------+-----------------+----------------------------------------------------+
            | *CHANPOREINDEX*       | *double*        | Pore index parameter in channel                    |
            +-----------------------+-----------------+----------------------------------------------------+
            | *CHANPSIB*            | *double*        | Matric potential in channel                        |
            +-----------------------+-----------------+----------------------------------------------------+
            | *OPTMESHINPUT*        | *int*           | Option for Mesh generation                         |
            +-----------------------+-----------------+----------------------------------------------------+
            | *RAINSOURCE*          | *int*           | Source of rainfall data                            |
            +-----------------------+-----------------+----------------------------------------------------+
            | *OPTEVAPOTRANS*       | *int*           | Option for Evapotranspiration scheme               |
            +-----------------------+-----------------+----------------------------------------------------+
            | *OPTSNOW*             | *int*           | Option for snow                                    |
            +-----------------------+-----------------+----------------------------------------------------+
            | *HILLALBOPT*          | *int*           | Option for hillslope albedo                        |
            +-----------------------+-----------------+----------------------------------------------------+
            | *OPTRADSHELT*         | *int*           | Option for radiation sheltering of snow            |
            +-----------------------+-----------------+----------------------------------------------------+
            | *OPTINTERCEPT*        | *int*           | Option for Interception scheme                     |
            +-----------------------+-----------------+----------------------------------------------------+
            | *OPTLANDUSE*          | *int*           | Option for static or dynamic land cover            |
            +-----------------------+-----------------+----------------------------------------------------+
            | *OPTLUINTERP*         | *int*           | Option for land cover interpolation                |
            +-----------------------+-----------------+----------------------------------------------------+
            | *OPTSOILTYPE*         | *int*           | Option for soil parameter format                   |
            +-----------------------+-----------------+----------------------------------------------------+
            | *GFLUXOPTION*         | *int*           | Option for Ground heat flux scheme                 |
            +-----------------------+-----------------+----------------------------------------------------+
            | *METDATAOPTION*       | *int*           | Point or Grid weather data                         |
            +-----------------------+-----------------+----------------------------------------------------+
            | *OPTBEDROCK*          | *int*           | Option for uniform or variable depth               |
            +-----------------------+-----------------+----------------------------------------------------+
            | *OPTGROUNDWATER*      | *int*           | Option to turn on groundwater module               |
            +-----------------------+-----------------+----------------------------------------------------+
            | *OPTGWFILE*           | *int*           | Option for groundwater input file                  |
            +-----------------------+-----------------+----------------------------------------------------+
            | *WIDTHINTERPOLATION*  | *int*           | Option for interpolating width variables           |
            +-----------------------+-----------------+----------------------------------------------------+
            | *OPTRUNON*            | *int*           | Option for hillslope runon                         |
            +-----------------------+-----------------+----------------------------------------------------+
            | *OPTRESERVOIR*        | *int*           | Option for reservoir routing                       |
            +-----------------------+-----------------+----------------------------------------------------+
            | *OPTPERCOLATION*      | *int*           | Option for channel percolation losses              |
            +-----------------------+-----------------+----------------------------------------------------+
            | *CENTROIDLAT*         | *double*        | Basin centroid latitude (solar position)           |
            +-----------------------+-----------------+----------------------------------------------------+
            | *CENTROIDLONG*        | *double*        | Basin centroid longitude (solar position)          |
            +-----------------------+-----------------+----------------------------------------------------+
            | *UTCOFFSET*           | *int*           | UTC offset in hours (solar position)               |
            +-----------------------+-----------------+----------------------------------------------------+
            | *INPUTDATAFILE*       | *base pathname* | Input file base name for Mesh files                |
            +-----------------------+-----------------+----------------------------------------------------+
            | *POINTFILENAME*       | *pathname*      | Input file name for Points files                   |
            +-----------------------+-----------------+----------------------------------------------------+
            | *SOILTABLENAME*       | *pathname*      | Soil parameter reference table                     |
            +-----------------------+-----------------+----------------------------------------------------+
            | *SOILMAPNAME*         | *pathname*      | Soil texture ASCII grid                            |
            +-----------------------+-----------------+----------------------------------------------------+
            | *LANDTABLENAME*       | *pathname*      | Land use parameter reference table                 |
            +-----------------------+-----------------+----------------------------------------------------+
            | *RSPARAMFILE*         | *pathname*      | Optional monthly stomatal resist. scaling          |
            +-----------------------+-----------------+----------------------------------------------------+
            | *LANDMAPNAME*         | *pathname*      | Land use ASCII grid                                |
            +-----------------------+-----------------+----------------------------------------------------+
            | *GWATERFILE*          | *pathname*      | Ground water ASCII grid                            |
            +-----------------------+-----------------+----------------------------------------------------+
            | *DEMFILE*             | *pathname*      | DEM for sheltering                                 |
            +-----------------------+-----------------+----------------------------------------------------+
            | *RAINFILE*            | *base pathname* | Radar Rainfall ASCII grids                         |
            +-----------------------+-----------------+----------------------------------------------------+
            | *RAINEXTENSION*       | *extension*     | Extension for Radar Rainfall grids                 |
            +-----------------------+-----------------+----------------------------------------------------+
            | *DEPTHTOBEDROCK*      | *meters*        | Uniform depth to bedrock                           |
            +-----------------------+-----------------+----------------------------------------------------+
            | *SURFACESOILDEPTH*    | *millimeters*   | Optional surface soil moisture depth               |
            +-----------------------+-----------------+----------------------------------------------------+
            | *BEDROCKFILE*         | *pathname*      | Bedrock depth ASCII grid                           |
            +-----------------------+-----------------+----------------------------------------------------+
            | *LUGRID*              | *pathname*      | Dynamic land cover ASCII grid list                 |
            +-----------------------+-----------------+----------------------------------------------------+
            | *SCGRID*              | *pathname*      | Soil parameter ASCII grid list                     |
            +-----------------------+-----------------+----------------------------------------------------+
            | *HYDROMETSTATIONS*    | *pathname*      | Hydrometeorological station file                   |
            +-----------------------+-----------------+----------------------------------------------------+
            | *HYDROMETGRID*        | *pathname*      | Hydrometeorological ASCII grid list                |
            +-----------------------+-----------------+----------------------------------------------------+
            | *GAUGESTATIONS*       | *pathname*      | Rain gauge station file                            |
            +-----------------------+-----------------+----------------------------------------------------+
            | *RESPOLYGONID*        | *pathname*      | Reservoir polygon ID file                          |
            +-----------------------+-----------------+----------------------------------------------------+
            | *RESDATA*             | *pathname*      | Reservoir data table                               |
            +-----------------------+-----------------+----------------------------------------------------+
            | *OUTFILENAME*         | *base pathname* | Mesh, spatial, and hydrograph output               |
            +-----------------------+-----------------+----------------------------------------------------+
            | *DYNVARFILE*          | *pathname*      | Optional dynamic output column list                |
            +-----------------------+-----------------+----------------------------------------------------+
            | *OPINTRVL*            | *hours*         | Output interval                                    |
            +-----------------------+-----------------+----------------------------------------------------+
            | *SPOPINTRVL*          | *hours*         | Spatial output interval                            |
            +-----------------------+-----------------+----------------------------------------------------+
            | *NODEOUTPUTLIST*      | *pathname*      | Node output list file                              |
            +-----------------------+-----------------+----------------------------------------------------+
            | *HYDRONODELIST*       | *pathname*      | Node runtime output list file                      |
            +-----------------------+-----------------+----------------------------------------------------+
            | *OUTLETNODELIST*      | *pathname*      | Interior node output list                          |
            +-----------------------+-----------------+----------------------------------------------------+
            | *OPTSPATIAL*          | *int*           | Option for generating spatial output               |
            +-----------------------+-----------------+----------------------------------------------------+
            | *TLINKE*              | *double*        | Atmospheric turbidity parameter                    |
            +-----------------------+-----------------+----------------------------------------------------+
            | *TEMPLAPSE*           | *double*        | Temperature lapse rate                             |
            +-----------------------+-----------------+----------------------------------------------------+
            | *PRECLAPSE*           | *double*        | Precipitation lapse rate                           |
            +-----------------------+-----------------+----------------------------------------------------+
            | *SNOWFILENAME*        | *pathname*      | Snow parameter file (.spf)                         |
            +-----------------------+-----------------+----------------------------------------------------+
            | *PARALLELMODE*        | *int*           | Run as serial (0) or parallel (1) mode             |
            +-----------------------+-----------------+----------------------------------------------------+
            | *GRAPHOPTION*         | *int*           | Option for graph file type (0, 1 or 2)             |
            +-----------------------+-----------------+----------------------------------------------------+
            | *GRAPHFILE*           | *filename*      | Reach connectivity (graph) filename                |
            +-----------------------+-----------------+----------------------------------------------------+
            | *RESTARTMODE*         | *int*           | Option for restart mode (0, 1, 2 or 3)             |
            +-----------------------+-----------------+----------------------------------------------------+
            | *RESTARTINTRVL*       | *hours*         | Time set for restart output                        |
            +-----------------------+-----------------+----------------------------------------------------+
            | *RESTARTDIR*          | *pathname*      | Path of directory for restart output               |
            +-----------------------+-----------------+----------------------------------------------------+
            | *RESTARTFILE*         | *filename*      | Filename of restart file                           |
            +-----------------------+-----------------+----------------------------------------------------+

Input File Options
------------------

Model Run Parameters: Time Variables
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The *STARTDATE* keyword is used to indicate the starting time of the model simulation in the following format: ``MM/DD/YYYY/HH/MM`` (Month/Day/Year/Hour/Minutes). Values of the rainfall and meteorological inputs must exist for this starting date for the model to execute properly. The *RUNTIME* keyword is used to specify the number of hours in the total length of the simulation. Similarly, there must be hydrometeorologic data that span the period between the start date and the end date. The *TIMESTEP* parameter is used to specify the Unsaturated Zone computational time step in minutes. For proper execution, the unsaturated time step should be on the order of minutes, default is 3.75 minutes. The *GWSTEP* represents the groundwater or Saturated Zone computational time step in minutes, default is 30 minutes. Typically, the groundwater time step can be on the order of tens of minutes for most applications. *METSTEP* specifies the time step of hydrometeorological data input from weather stations, in minutes. This time step is usually set to 60 minutes since the weather parameters are available at this temporal resolution. The *RAININTRVL* keyword is used for the input time interval of rainfall data, either from radar rainfall grids or from raingauges, in hours. This interval will depend on the resolution of the radar data which is available from 15 minute up to daily intervals. *INTSTORMMAX* is the amount of hours without rainfall that the model considers to be sufficient for an interstorm period to begin, while *RAINSEARCH* is the amount of hours that the model will search for the next rainfall file without producing an error message and exiting the program.

Model Input Files and Pathnames: Mesh Generation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The *OPTMESHINPUT* keyword is used to indicate the option for inputting the topographic data into the model. It controls the sort of mesh data that is read by the model and necessary input data files related to the model mesh. As of v6.0.0 two options are implemented within the tRIBS Model: 1 = tMesh files from a generated using pytRIBS or output from a prior run are used to recreate the mesh (``*.nodes``, ``*.edges``, ``*.tri``, ``*.z``); 2 = Point File used with Tipper Triangulation procedure (``*.points``).

When specifying the *OPTMESHINPUT* option, the model will require that the pathname of the input files be included within the Mesh Generation section of the Model Input File. The *INPUTDATAFILE* option is used to input the basename for the Mesh input files produced during a previous run (*OPTMESHINPUT = 1*). If using *OPTMESHINPUT = 2*, then the *POINTFILENAME* keyword must be used to specify the pathname and filename of the Points File (``*.points``).

Model Run Parameters: Routing Variables
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The tRIBS has a simplified hydrologic scheme for hillslope routing and a finite-element channel routing scheme. The model allows for non-linear routing based on the discharge at a single watershed outlet and two parameter values, the stream velocity and the hillslope velocity, shared by all TIN nodes of that particular type. The hydrologic routing scheme utilizes the discharge at the closest stream node to determine the hillslope velocity. Six routing parameters are specified to the model: *BASEFLOW*, *VELOCITYCOEF*, *FLOWEXP*, *VELOCITYRATIO*, *CHANNELROUGHNESS*, and *CHANNELWIDTH*. (Note: The *WIDTHINTERPOLATION* keyword is used to specify whether or not channel widths will be interpolated between the measured and observed widths (= 0) or only between the measured channel widths (= 1), inputted to the model through the file name specified using the keyword *CHANNELWIDTHFILE*.)

*BASEFLOW* is used to specify the minimum flow in the stream network in cubic meters per second, a required parameter since the flow network velocities depend on the outlet discharge in some linear or nonlinear fashion. If the *BASEFLOW* parameter is not specified, a value of 0.001 cubic meters per second is assigned as default. *VELOCITYCOEF* is used to specify the coefficient in the relationship between the stream velocity and the outlet discharge, while the *FLOWEXP* is the exponent on the discharge in this relationship. Specifying *FLOWEXP = 0* implies a linear relationship between the stream velocity and the outlet discharge. The *VELOCITYRATIO* keyword is the ratio between the calculated stream velocity and the hillslope velocity assigned to non-stream nodes. The last two parameters: *CHANNELROUGHNESS* and *CHANNELWIDTH* are both uniform parameters for the entire stream network in this model version. The roughness parameters refers to a non-dimensional Manning's coefficient while the width is a channel width in meters.

The tRIBS model uses a simplified hydrologic scheme for hillslope routing and a finite-element channel routing scheme. The model allows for non-linear routing in which travel velocities depend on discharge, governed by a velocity coefficient and exponent that are shared by all TIN nodes. For each non-stream (hillslope) node, the routing velocity is determined from the discharge at the closest stream node. Five routing parameters are specified to the model: BASEFLOW, KINEMVELCOEF, FLOWEXP, CHANNELROUGHNESS, and CHANNELWIDTH. (Note: The WIDTHINTERPOLATION keyword specifies whether channel widths are interpolated between the measured and computed widths (= 0) or only between the measured channel widths (= 1), supplied through the file named with the CHANNELWIDTHFILE keyword.)

BASEFLOW specifies the minimum flow in the stream network in cubic meters per second. It is required because the flow-network velocities depend on discharge, so a nonzero lower bound is needed to keep velocities defined at low flow, if BASEFLOW would otherwise be zero while FLOWEXP is nonzero, the model resets it to 0.1 cubic meters per second. KINEMVELCOEF is the coefficient in the power-law relationship between velocity and specific discharge (discharge per unit contributing area), and FLOWEXP is the exponent in that relationship. Setting FLOWEXP = 0 yields a constant velocity equal to KINEMVELCOEF, independent of discharge, while a positive FLOWEXP produces velocities that scale non-linearly with discharge. The remaining two parameters, CHANNELROUGHNESS and CHANNELWIDTH, are uniform across the entire stream network: the roughness is a non-dimensional Manning's coefficient and the width is a channel width in meters. (A spatially variable channel width may instead be derived from a power law of contributing area using the CHANNELWIDTHCOEFF and CHANNELWIDTHEXPNT keywords.)


Model Run Parameters: Hydrologic Processes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This section is dedicated to the model run options used to specify which hydrological processes are chosen for a particular model run.

The *OPTEVAPOTRANS* parameter indicates the evapotranspiration option selected during the model run. The choice of the particular option will set the required parameter values used from the land use reclassification table and the meteorological data file. Three options are available for evapotranspiration: 0 = Inactive; 1 = Penman-Monteith method; 2 = Pan Evaporation measurements. The *OPTINTERCEPT* option allows the user to choose between two interception routines: 0 = Inactive, 1 = Rutter canopy water balance method. The *GFLUXOPTION* keyword allows two types of ground heat flux calculations to be performed: 0 = Inactive; 1 = Temperature gradient method; 2 = Force-restore method. The choice of the particular option will set the required parameter values used from the soil reclassification table. The optional *RSPARAMFILE* keyword is a recently added feature outlined in `(Becerra, 2026)`_. The feature allows a set of 12 monthly multipliers that scale the minimum stomatal resistance seasonally, on top of the built-in diurnal scaling. If the keyword is absent, all monthly factors default to 1.0 and behavior is unchanged from previous versions. See :doc:`Model_Parameters_Forcings` for the file format.

The *OPTSNOW* parameter indicates the snow pack option used. Currently, either the single-layer energy balance module is on (*OPTSNOW = 1*) or off (*OPTSNOW = 0*). With the single-layer EB model, a recent addition to tRIBS include the ability to read a snow parameter file (``*.spf``) controlled by the keyword *SNOWFILENAME*. This file contains paramters that were previously hardcoded in tRIBS. The model will default to standard values if no file is provided or the `tRIBS Benchmarks repository`_ contains a pre-prepared file to start from. See :doc:`Model_Parameters_Forcings` for more details on the parameters and file structure.

.. _tRIBS Benchmarks repository: https://github.com/tRIBS-Model/tRIBS-benchmarks
.. _(Becerra, 2026): https://hdl.handle.net/2286/R.2.N.204771

The *OPTLANDUSE* parameter is used to indicate the type of land cover maps used in the simulation. Three options exist: *OPTLANDUSE = 0* (static representation read from the land use table at the initial time period), *OPTLANDUSE = 1* (dynamic updating of the land cover at times specified by the available time-stamped grids), and *OPTLANDUSE = 2* (spatially variable but temporally constant land use read from a single, non-time-stamped grid per parameter). If the *OPTLANDUSE = 1 or 2* is specified, then the user must indicate the pathname of the file containing the filenames of the ASCII grids to be read. This is specified using the keyword LUGRID (pathname to a Grid Data File, ``*.gdf``). This file should contain the pathnames to the gridded land cover rasters. File naming convention only uses up to the hourly time stamp (no minutes, for example). For the dynamic, time varying option the files need to be within the time boundaries of the simulation period. The keyword *OPTLUINTERP* allows for two types of interpolation between available land cover maps (at different time periods). *OPTLUINTERP = 0* assigns the current gridded time step value to all model time steps up until the next available file. *OPTLUINTERP = 1* linearly interpolates the land cover parameter values between two different grid time steps.

The *OPTSOILTYPE* keyword is used to activate the use of gridded soil parameter data input into the model. This option replaces the use of a soil grid index map and a soil parameter table. Two options exist: *OPTSOILTYPE = 0*, uses the traditional tabular soil data associated with a soil map of soil type numbers; *OPTSOILTYPE =1*, activates the use of gridded soil data. If *OPTSOILTYPE = 1* a Grid Data File (``*.gdf``) file indicating the paths to all the grid files for each soil parameter. The format is similar to that used for the dynamic land cover maps. The path to the ``*.gdf``  is indicated under *SCGRID* keyword in the Input File.

The *OPTBEDROCK* keyword is used to specify the format of the bedrock depth data: 0 = Uniform bedrock depth over the basin; 1 = Grid bedrock file. If *OPTBEDROCK = 0*, then the *DEPTHTOBEDROCK* keyword is required (input is a double), otherwise the *BEDROCKFILE* keyword is required (input is a path and filename with extension ``*.brd``).

The *OPTGROUNDWATER* runs the groundwater module; 1 = the groundwater module is on, 0= the groundwater module is off.

The *OPTGWFILE* keyword is used to specify the format of the initial groundwater input file. 0 = Resample ASCII grid file indicated in *GWATERFILE*; 1 = Read in Voronoi polygon file with groundwater levels output from previous run. *GWATERFILE* keyword only used for *OPTGWFILE* option 0, otherwise, the Voronoi GW file is read in through user interaction with model run (e.g. through screen).


The *OPTRESERVOIR* keyword is used to activate the use of the linear reservoir module (``tReservoir``) in the model. 0 = Disable the use of Reservoirs. 1 = Activate the use of Reservoirs. If *OPTRESERVOIR = 1* then additional information is required by specifying the path to the file containing the TIN nodes (or Voronoi polygons) to be used as reservoirs in *RESPOLYGONID* (``*.res``) and the path to the file containing the elevation-discharge-storage information for each type of reservoir in *RESDATA* (``*.eds``).

The *OPTPERCOLATION* keyword allows the user to select from several options for channel percolation losses. 0 = No channel percolation. 1 = Constant loss method where the infiltration rate is equal to the channel saturated hydraulic conductivity specified under *CHANNELCONDUCTIVITY* (in *mm/hr*). 2 = Constant loss method with a transient period applied with the transient hydraulic conductivity specified as *TRANSIENTCONDUCTIVITY* (in *mm/hr*) and the transient time period specified as *TRANSIENTTIME*. Only the parameters required by the selected option need to be provided, and the channel conductivity parameters are expected in *mm/hr*. Option 3 (Green-Ampt) has been temporarily disabled: the model will exit with a message directing users to options 1 or 2, and it will be re-enabled in a future release.


Mesh Input Files and Pathnames: Spatial Data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The path and filenames of the grid input and the reclassification tables for the soil and land use data are grouped together within this section of the Input File. The soil grid (``*.soi``), land use grid (``*.lan``) and initial groundwater table position (``*.iwt``) are specified using the *SOILMAPNAME*, *LANDMAPNAME* and *GWATERFILE* keywords. The DEM used to derive the remote sheltering grids is specified by the *DEMFILE* keyword. The DEM used should encompass the study area and all significant surrounding topographic features, possibly outside the study area.

Additionally if *OPTBEDROCK* =1, then the path and filename with extension to the ascii grid must be specified using *BEDROCKFILE*. Likewise if either OPTSOILTYPE = 1 and or *OPTLANDUSE* = 1 then a grid data file (*.gdf) will need to be specified with the keywords *SCGRID* and *LUGRID*, respectively.


Mesh Input Files and Pathnames: Meteorological Data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The path and filenames of the meteorological data are grouped together in this section of the Model Input File.

The *RAINSOURCE* keyword is used to indicate the rainfall data source given to the model. As of v6.0.0 two sources are considered: 1 = gridded (radar) rainfall data in *mm/hr*; 2 = rain gauge station data in *mm/hr*.The gridded rainfall grid (for *RAINSOURCE = 1*) base name is specified using the *RAINFILE* keyword and the extension is inputted by using the *RAINEXTENSION* keyword.


The *METDATAOPTION* is used to indicated the input format for the meteorological data: 0 = Inactive; 1 = Weather station point data; 2 = Grid meteorological data. The particular choice determines which type of text files, grid or point data files are required during model execution.

If *METDATAOPTION = 1*, then the Station Data File (``*.sdf``) must be specified in *HYDROMETSTATIONS*; each Meteorological Data File (``*.mdf``) is referenced from within the SDF via its ``DataFile`` column. Otherwise, if *METDATAOPTION = 2*, then the *HYDROMETGRID* keyword must contain the Grid Data File (``*.gdf``). If *RAINSOURCE = 2* (rain gauge data), then the *GAUGESTATIONS* keyword is used to specify the rain gauge SDF file.

When evapotranspiration is enabled (*OPTEVAPOTRANS != 0*), the basin centroid latitude, longitude, and UTC offset used for solar-position calculations must be provided in the main input file via the *CENTROIDLAT*, *CENTROIDLONG*, and *UTCOFFSET* keywords. These keywords control only the solar geometry and do not shift the input time series.

Lapse rates have been implemented in the model for precipitation and temperature. The temperature lapse rate is assigned from *TEMPLAPSERATE*. The precipitation lapse rate is specified by *PRECLAPSE* in *mm/m*. Scattered light from opposing hillslopes can be a significant component of incoming radiations in snowy environments. *HILLALBOPT = 0* uses the snow albedo for the hillslope albedo, *HILLALBOPT = 1* uses the land-use albedo for the hillslope albedo, and *HILLALBOPT = 2* uses a dynamic representation of albedo, where the snow albedo is used if there is snow in the canopy and a vegetative fraction weighted average of snow and land-use albedo is used otherwise. *OPTRADSHELT*  tells what radiation sheltering scheme is used: 0 = local; 1 = remote controls on diffuse shortwave radiation; 2 = remote controls on entire shortwave radiation; 3 = no sheltering.


Mesh Input Files and Pathnames: Output Data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The path and basenames of the output data are grouped in this section of the Model Input File. All output: mesh, spatial, and hydrograph/time-series files share a single base path specified by the *OUTFILENAME* keyword. This base path is used for the output mesh and Voronoi files (``*.nodes``, ``*.tri``, ``*.edges``, ``*.z`` and ``*.voi``), the dynamic variable output (``*.pixel``), and the hydrograph and mean-response time series (``*.qout``, ``*.mrf``). The *NODEOUTPUTLIST* keyword specifies the Node Output List (``*.nol``) file used to select nodes for ``*.pixel`` output, and the *OUTLETNODELIST* keyword specifies interior stream nodes for interior hydrograph (``*.qout``) output. As of v6.0.0 these node-list files may specify locations either by node ID or by *X,Y* coordinate (see :doc:`Model_Input_Formats`). The optional *DYNVARFILE* keyword accepts a single-row CSV listing which dynamic output columns to write; if it is omitted, all columns are written. *OPINTRVL* notifies the model at what interval the time-series output is produced, while *SPOPINTRVL* specifies the interval at which the dynamic spatial files are written out.

Model Modes: Restart Mode
^^^^^^^^^^^^^^^^^^^^^^^^^

The tRIBS model can write a binary restart (hotstart) file capturing the model state, which can be used to resume a simulation or to provide initial conditions for a later run (for example, spin-up initialization). The restart mechanism is invoked with the *RESTARTMODE* keyword, which has four options: no restart (0), write files only (1), read files only (2), read and write files (3). When writing, restart files are produced at the interval set by *RESTARTINTRVL* (in hours) in the directory given by *RESTARTDIR*; when reading, the initial state is taken from the file given by *RESTARTFILE*.

As of v6.0.0 the restart module was rewritten to save only the state variables required to resume a simulation, producing more widely usable and smaller files. A restart file is independent of the simulation time/date and, in parallel mode, is written as a single file that can be read by any number of processors (the previous constraint that write-time and read-time processor counts match has been removed). Restart files written by earlier versions of tRIBS are not compatible with v6.0.0. Note that restarting mid-storm may produce a short divergence in outlet streamflow due to water in transit in the hillslope routing queues.

Model Modes: Parallel Mode
^^^^^^^^^^^^^^^^^^^^^^^^^^

The tRIBS model can be run in either serial or parallel mode. The keyword *PARALLELMODE* is used to specify either serial (option 0) or parallel (option 1) computation. If the parallel mode is used, then attention needs to be paid to the graph file partitioning option. Three methods for graph partitioning can be selected utilizing the keyword *GRAPHOPTION*: (a) A default partitioning of the graph (option 0); (b) A reach-based partitioning (option 1); and (c) An inlet/outlet-based partitioning (option 2). If either option 1 or 2 are selected, the keyword *GRAPHFILE* needs to be specified with the name of the graph file to be used (either reach or inlet/outlet based). Otherwise, no filename is required. The most commonly used graph partitioning option is option 1, default partitioning is largely for demonstration only and will likely produce slower simulations than serial mode.
