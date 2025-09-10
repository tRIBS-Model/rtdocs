Model Outputs
==================================

The tRIBS Model produces a number of output files that represent the time series or the spatial distribution of model state or output variables. Output variables include the position of moisture fronts in the unsaturated zone, water table elevation, surface runoff, subsurface flux, rainfall rate, interception loss, evapotranspiration, and information on the mesh triangulation. **Table 6.1**, **Table 6.2**, and **Table 6.3** summarize: (1) mesh output files, (2) time series outputs, and (3) spatial outputs. More detailed descriptions of the individual files are provided in the following sections.

    **Table 6.1** tRIBS Mesh Output Files

            .. tabularcolumns::  |c|c|l|

            +------------------------------+------------------+----------------------------------------------------------------+
            | Model Mesh Files             |  Extension       |  Description                                                   |
            +==============================+==================+================================================================+
            |*Mesh Node File*              |  ``*.nodes``     |  Node (x,y), ID of spoke, boundary code.                       |
            +------------------------------+------------------+----------------------------------------------------------------+
            |*Mesh Edge File*              |  ``*.edges``     |  ID of origin and destination node, ID of CCW edge.            |
            +------------------------------+------------------+----------------------------------------------------------------+
            |*Mesh Triangle File*          |  ``*.tri``       |  ID of vertex nodes, ID of neighboring triangles opposite the  |
            |                              |                  |  vertex node, ID of CCW edge originating with the vertex node. |
            +------------------------------+------------------+----------------------------------------------------------------+
            |*Mesh Node Elevation File*    | ``*.z``          |  Node elevation (meters).                                      |
            +------------------------------+------------------+----------------------------------------------------------------+
            |*Mesh Voronoi Geometry*       | ``*_voi``        |  File containing individual Voronoi polygon geometry.          |
            +------------------------------+------------------+----------------------------------------------------------------+

    **Table 6.2** tRIBS Model Time Series Files

            .. tabularcolumns::  |c|c|l|

            +------------------------------+------------------+----------------------------------------------------------------+
            | Time Series                  |  Extension       | Description                                                    |
            +==============================+==================+================================================================+
            |*Discharge Time Series*.      |``*_Outlet.qout`` | Time series of outlet or node hydrograph (m³/s).               |
            |                              |  ``or *.qout``   |                                                                |
            +------------------------------+------------------+----------------------------------------------------------------+
            |*Basin Averaged File*         |  ``*.mrf``       | Time series of basin-averaged model variables.                 |
            +------------------------------+------------------+----------------------------------------------------------------+
            |*Hydrograph Runoff Types File*|  ``*.rft``       | Time series of outlet hydrograph by runoff type (m³/s).        |
            +------------------------------+------------------+----------------------------------------------------------------+
            |*Node Dynamic Output File*    |  ``*.pixel``     | Time series of dynamic variables for a specific node.          |
            +------------------------------+------------------+----------------------------------------------------------------+

    **Table 6.3** tRIBS Model Spatial Output Files

            .. tabularcolumns::  |c|c|l|

            +------------------------------+------------------+----------------------------------------------------------------+
            |Model Spatial Output Files    |  Extension       |  Description                                                   |
            +==============================+==================+================================================================+
            |*Mesh Dynamic Output File*    |``*timestamp_00d``|  Dynamic variable output for all mesh nodes at specific time.  |
            +------------------------------+------------------+----------------------------------------------------------------+
            |*Mesh Integrated Output File* |``*timestamp_00i``|  Time-integrated variable output for all mesh nodes.           |
            +------------------------------+------------------+----------------------------------------------------------------+

    The location of the output files is specified in the tRIBS Model Input File by using the keywords *OUTFILENAME* and *OUTHYDROFILENAME*. An important note to make is that the ``*.mrf``, ``*.rft`` and ``*.dat`` files produced by the model are labeled with additional identifiers before the extension that relate to the time of the output. For each *OPINTRVL* time step, the model will produce output of the ``*.mrf`` type, while the ``*.rft`` file is produced only after completion of the entire run. The spatial output (``*timestamp_00d``) are determined by the time step specified in the *SPOPINTRVL* keyword. Time-integrated spatial output (``*timestamp_00i``) is produced only at the end of the simulation. The model also produces various files with a ``*.pixel`` extension followed by a node ID number at the end of the run. The ``*.pixel#`` files contain the dynamic variable output for a single node for all model times. The number of ``*.pixel#`` files produced is specified through a Node Output List (``*.nol``) File described below.

    **Table 6.4** Node Output List File Structure

            .. tabularcolumns:: |c|

            +-----------+
            | *#Nodes*  |
            +-----------+
            | *NodeID 1*|
            +-----------+
            | *...*     |
            +-----------+
            | *NodeID n*|
            +-----------+


    A similar structure and file is used for the keyword *HYDRONODELIST* and *OUTLETNODELIST*. Using this file, allows the user to obtain the runtime hydrologic information in the unsaturated and saturated model for each time step as output to the screen, a useful tool for debugging. No filename suppresses the debugging information.

Time Series
-----------

Basin Outlet Discharge Time Series
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  **Table 6.5** Content of *_Outlet.qout file or *qout file if Voronoi IDs are provided via OUTLETNODELIST

        .. tabularcolumns:: |c|c|c|

        +-------+-------------------+--------+
        | Column| Description       | Units  |
        +=======+===================+========+
        | 1     | Time              | [hr]   |
        +-------+-------------------+--------+
        | 2     | Discharge, Qstrm  |[m3/s]  |
        +-------+-------------------+--------+
        | 3     | Channel stage,    | [m]    |
        |       | HLevel            |        |
        +-------+-------------------+--------+

Hydrologic Time Series at Selected TIN nodes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  **Table 6.6** Content of *.pixel files

        .. tabularcolumns:: |c|c|c|

        +-------+--------------------------------------------+--------+
        | Column| Description                                | Units  |
        +=======+============================================+========+
        | 1     | Node Identification, ID                    | [id]   |
        +-------+--------------------------------------------+--------+
        | 2     | Time                                       | [hr]   |
        +-------+--------------------------------------------+--------+
        | 3     | Depth to groundwater table, Nwt            | [mm]   |
        +-------+--------------------------------------------+--------+
        | 4     | Wetting front depth, Nf                    | [mm]   |
        +-------+--------------------------------------------+--------+
        | 5     | Top front depth, Nt                        | [mm]   |
        +-------+--------------------------------------------+--------+
        | 6     | Total moisture above the water table, Mu   | [mm]   |
        +-------+--------------------------------------------+--------+
        | 7     | Moisture content in the initialization     | [mm]   |
        |       | profile, Mi                                |        |
        +-------+--------------------------------------------+--------+
        | 8     | Unsaturated lateral flow out from cell,    | [mm/hr]|
        |       | Qpout                                      |        |
        +-------+--------------------------------------------+--------+
        | 9     | Unsaturated lateral flow into cell, Qpin   | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 10    | Transmissivity, Trnsm                      | [m²/hr]|
        +-------+--------------------------------------------+--------+
        | 11    | Groundwater flux, GWflx                    | [m³/hr]|
        +-------+--------------------------------------------+--------+
        | 12    | Surface Runoff, Srf                        | [mm]   |
        +-------+--------------------------------------------+--------+
        | 13    | Rainfall, Rain                             | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 14    | Soil Moisture, top 10 cm, SoilMoist        | [ ]    |
        +-------+--------------------------------------------+--------+
        | 15    | Root Zone Moisture, top 1 m, RootMoist     | [ ]    |
        +-------+--------------------------------------------+--------+
        | 16    | Air Temperature, AirT                      | [°C]   |
        +-------+--------------------------------------------+--------+
        | 17    | Dew Point Temperature, DewT                | [°C]   |
        +-------+--------------------------------------------+--------+
        | 18    | Surface Temperature, SurfT                 | [°C]   |
        +-------+--------------------------------------------+--------+
        | 19    | Soil Temperature, SoilT                    | [°C]   |
        +-------+--------------------------------------------+--------+
        | 20    | Atmospheric Pressure, Press                | [Pa]   |
        +-------+--------------------------------------------+--------+
        | 21    | Relative Humidity, RelHum                  | [ ]    |
        +-------+--------------------------------------------+--------+
        | 22    | Sky Cover, SkyCov                          | [ ]    |
        +-------+--------------------------------------------+--------+
        | 23    | Wind Speed, Wind                           | [m/s]  |
        +-------+--------------------------------------------+--------+
        | 24    | Net Radiation, NetRad                      | [W/m²] |
        +-------+--------------------------------------------+--------+
        | 25    | Incoming Shortwave Radiation, ShrtRadIn    | [W/m²] |
        +-------+--------------------------------------------+--------+
        | 26    | Incoming Shortwave Radiation to the Sloped | [W/m²] |
        |       | Surface, ShrtRadInslope                    |        |
        +-------+--------------------------------------------+--------+
        | 27    | Incoming Direct Shortwave Radiation,       | [W/m²] |
        |       | ShrtRadIn_dir                              |        |
        +-------+--------------------------------------------+--------+
        | 28    | Incoming Diffuse Shortwave Radiation,      | [W/m²] |
        |       | ShrtRadIn_dif                              |        |
        +-------+--------------------------------------------+--------+
        | 29    | Shortwave Absorbed Radiation, Vegetation,  | [W/m²] |
        |       | ShortAbsbVeg                               |        |
        +-------+--------------------------------------------+--------+
        | 30    | Shortwave Absorbed Radiation, Soil,        | [W/m²] |
        |       | ShortAbsbSoi                               |        |
        +-------+--------------------------------------------+--------+
        | 31    | Incoming Longwave Radiation, LngRadIn      | [W/m²] |
        +-------+--------------------------------------------+--------+
        | 32    | Outgoing Longwave Radiation, LngRadOut     | [W/m²] |
        +-------+--------------------------------------------+--------+
        | 33    | Potential Evaporation, PotEvp              | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 34    | Actual Evaporation, ActEvp                 | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 35    | Total Evapotranspiration, EvpTtrs          | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 36    | Evaporation from Wet Canopy, EvpWetCan     | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 37    | Evaporation from Dry Canopy,               | [mm/hr]|
        |       | EvpDryCan                                  |        |
        +-------+--------------------------------------------+--------+
        | 38    | Evaporation from Bare Soil, EvpSoil        | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 39    | Ground Heat Flux, Gflux                    | [W/m²] |
        +-------+--------------------------------------------+--------+
        | 40    | Sensible Heat Flux, Hflux                  | [W/m²] |
        +-------+--------------------------------------------+--------+
        | 41    | Latent Heat Flux, Lflux                    | [W/m²] |
        +-------+--------------------------------------------+--------+
        | 42    | Net Precipitation, NetPrecip               | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 43    | Liquid Water Equivalent, LiqWE             | [cm]   |
        +-------+--------------------------------------------+--------+
        | 44    | Ice Water Equivalent, IceWE                | [cm]   |
        +-------+--------------------------------------------+--------+
        | 45    | Snow Water Equivalent, SnWE                | [cm]   |
        +-------+--------------------------------------------+--------+
        | 46    | Sublimation from Snowpack, SnSub           | [cm]   |
        +-------+--------------------------------------------+--------+
        | 47    | Evaporation from Snowpack, SnEvap          | [cm]   |
        +-------+--------------------------------------------+--------+
        | 48    | Internal Energy of Snow Pack, U            | [kJ/m²]|
        +-------+--------------------------------------------+--------+
        | 49    | Routed Melt Water Equivalent, RouteWE      | [cm]   |
        +-------+--------------------------------------------+--------+
        | 50    | Snow Temperature, SnTemp                   | [°C]   |
        +-------+--------------------------------------------+--------+
        | 51    | Snow Surface Age, SurfAge                  | [hr]   |
        +-------+--------------------------------------------+--------+
        | 52    | Change in Snow Pack Internal Energy, DU    | [kJ/m²]|
        +-------+--------------------------------------------+--------+
        | 53    | Latent Heat Flux from Snow Cover, snLHF    | [kJ/m²]|
        +-------+--------------------------------------------+--------+
        | 54    | Sensible Heat Flux from Snow Cover, snSHF  | [kJ/m²]|
        +-------+--------------------------------------------+--------+
        | 55    | Ground Heat Flux from Snow Cover, snGHF    | [kJ/m²]|
        +-------+--------------------------------------------+--------+
        | 56    | Precip Heat Flux from Snow Cover, snPHF    | [kJ/m²]|
        +-------+--------------------------------------------+--------+
        | 57    | Outgoing Longw. Rad. from Snow, snRLout    | [kJ/m²]|
        +-------+--------------------------------------------+--------+
        | 58    | Incom. Longw. Radn. from Snow, snRLin      | [kJ/m²]|
        +-------+--------------------------------------------+--------+
        | 59    | Incom. Shortw. Radn. from Snow, snRSin     | [kJ/m²]|
        +-------+--------------------------------------------+--------+
        | 60    | Error in Energy Balance, Uerror            | [kJ/m²]|
        +-------+--------------------------------------------+--------+
        | 61    | Intercepted Snow Water Equivalent, intSWEq | [cm]   |
        +-------+--------------------------------------------+--------+
        | 62    | Sublim. Snow Water Equiv. from Canopy,     | [cm]   |
        |       | intSub                                     |        |
        +-------+--------------------------------------------+--------+
        | 63    | Unloaded SWE from Canopy, intSnUnload      | [cm]   |
        +-------+--------------------------------------------+--------+
        | 64    | Canopy Storage, CanStorage                 | [mm]   |
        +-------+--------------------------------------------+--------+
        | 65    | Cumulative Interception, CumIntercept      | [mm]   |
        +-------+--------------------------------------------+--------+
        | 66    | Interception, Interception                 | [mm]   |
        +-------+--------------------------------------------+--------+
        | 67    | Recharge, Recharge                         | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 68    | Runon, RunOn                               | [mm]   |
        +-------+--------------------------------------------+--------+
        | 69    | Surface Runoff in Hour, srf_Hour           | [mm]   |
        +-------+--------------------------------------------+--------+
        | 70    | Discharge, Qstrm                           | [m³/s] |
        +-------+--------------------------------------------+--------+
        | 71    | Channel Stage, Hlevel                      | [m]    |
        +-------+--------------------------------------------+--------+
        | 72    | Canopy Storage Parameter, CanStorParam     | [mm]   |
        +-------+--------------------------------------------+--------+
        | 73    | Interception Coefficient, IntercepCoeff    | [ ]    |
        +-------+--------------------------------------------+--------+
        | 74    | Free Throughfall Coeff.- Rutter,           | [ ]    |
        |       | ThroughFall                                |        |
        +-------+--------------------------------------------+--------+
        | 75    | Canopy Field Capacity – Rutter, CanFieldCap| [mm]   |
        +-------+--------------------------------------------+--------+
        | 76    | Drainage coefficient – Rutter, DrainCoeff  | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 77    | Drainage Expon. Param. – Rutter,           | [mm⁻¹] |
        |       | DrainExpPar                                |        |
        +-------+--------------------------------------------+--------+
        | 78    | Albedo, LandUseAlb                         | [ ]    |
        +-------+--------------------------------------------+--------+
        | 79    | Vegetation Height , VegHeight              | [m]    |
        +-------+--------------------------------------------+--------+
        | 80    | Optical Transmission Coeff., OptTransmCoeff| [ ]    |
        +-------+--------------------------------------------+--------+
        | 81    | Canopy- Average Stomatal Resistance,       | [s/m]  |
        |       | StomRes                                    |        |
        +-------+--------------------------------------------+--------+
        | 82    | Vegetation Fraction, VegFraction           | [ ]    |
        +-------+--------------------------------------------+--------+
        | 83    | Canopy Leaf Area Index, LeafAI             | [ ]    |
        +-------+--------------------------------------------+--------+

Basin-averaged Hydrological Time Series
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  **Table 6.7** Content of *.mrf file

        .. tabularcolumns:: |c|c|c|

        +-------+--------------------------------------------+--------+
        | Column| Description                                | Units  |
        +=======+============================================+========+
        | 1     | Time                                       | [hr]   |
        +-------+--------------------------------------------+--------+
        | 2     | Surface Runoff from Hydrologic Routing, Srf| [m³/s] |
        +-------+--------------------------------------------+--------+
        | 3     | Mean Areal Precipitation, MAP              | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 4     | Maximum Rainfall Rate, Max                 | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 5     | Minimum Rainfall Rate, Min                 | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 6     | Forecast State, Fstate                     | [ ]    |
        +-------+--------------------------------------------+--------+
        | 7     | Mean Surface Soil Moisture (in top 10 cm), | [ ]    |
        |       | MSM100                                     |        |
        +-------+--------------------------------------------+--------+
        | 8     | Mean Soil Moisture in Root Zone (in top 1  | [ ]    |
        |       | m), MSMRt                                  |        |
        +-------+--------------------------------------------+--------+
        | 9     | Mean Soil Moisture in Unsaturated Zone     | [ ]    |
        |       | (above water table), MSMU                  |        |
        +-------+--------------------------------------------+--------+
        | 10    | Mean Depth to Groundwater, MGW             | [mm]   |
        +-------+--------------------------------------------+--------+
        | 11    | Mean Evapotranspiration, MET               | [mm]   |
        +-------+--------------------------------------------+--------+
        | 12    | Areal Fraction of Surface Saturation, Sat  | [ ]    |
        +-------+--------------------------------------------+--------+
        | 13    | Areal Fraction of Rainfall, Rain           | [ ]    |
        +-------+--------------------------------------------+--------+
        | 14    | Average Snow Water Equivalent, AvSWE       | [cm]   |
        +-------+--------------------------------------------+--------+
        | 15    | Average Amount of Snow Melt, AvMelt        | [cm]   |
        +-------+--------------------------------------------+--------+
        | 16    | Average Snow Temperature, AvSTC            | [°C]   |
        +-------+--------------------------------------------+--------+
        | 17    | Average Change in Snow Pack Internal       | [kJ/m²]|
        |       | Energy, AvDUint                            |        |
        +-------+--------------------------------------------+--------+
        | 18    | Average Latent Heat Flux from Snow         | [kJ/m²]|
        |       | Covered Areas, AvSLHF                      |        |
        +-------+--------------------------------------------+--------+
        | 19    | Average Sensible Heat Flux from Snow       | [kJ/m²]|
        |       | Covered Areas, AvSSHF                      |        |
        +-------+--------------------------------------------+--------+
        | 20    | Average Precipitation Heat Flux from Snow  | [kJ/m²]|
        |       | Covered Areas, AvSPHF                      |        |
        +-------+--------------------------------------------+--------+
        | 21    | Average Ground Heat Flux from Snow         | [kJ/m²]|
        |       | Covered Areas, AvSGHF                      |        |
        +-------+--------------------------------------------+--------+
        | 22    | Average Incoming Longwave Radiation from   | [kJ/m²]|
        |       | Snow Covered Areas, AvSRLI                 |        |
        +-------+--------------------------------------------+--------+
        | 23    | Average Outgoing Longwave Radiation from   | [kJ/m²]|
        |       | Snow Covered Areas, AvSRLO                 |        |
        +-------+--------------------------------------------+--------+
        | 24    | Average Incoming Shortwave Radiation from  | [kJ/m²]|
        |       | Snow Covered Areas, AvSRSI                 |        |
        +-------+--------------------------------------------+--------+
        | 25    | Mean Intercepted Snow Water Equivalent,    | [cm]   |
        |       | AvInSn                                     |        |
        +-------+--------------------------------------------+--------+
        | 26    | Mean Sublimation from Intercepted Snow,    | [cm]   |
        |       | AvInSu                                     |        |
        +-------+--------------------------------------------+--------+
        | 27    | Mean Unloaded Snow from Canopy, AvInUn     | [cm]   |
        +-------+--------------------------------------------+--------+
        | 28    | Fraction Snow Covered Area, SCA            | [ ]    |
        +-------+--------------------------------------------+--------+
        | 29    | Channel percolation, ChanP                 | [m³]   |
        +-------+--------------------------------------------+--------+
        | 29    | Net Outflow from Unsaturated Zone, nQunsat | [mm]   |
        +-------+--------------------------------------------+--------+

Basin-averaged Hydrological Time Series
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  **Table 6.8** Content for *.rft files

        .. tabularcolumns:: |c|c|c|

        +-------+-----------------------------------+--------+
        | Column| Description                       | Units  |
        +=======+===================================+========+
        | 1     | Time                              | [hr]   |
        +-------+-----------------------------------+--------+
        | 2     | Infiltration-excess Runoff, Hsrf  | [m³/s] |
        +-------+-----------------------------------+--------+
        | 3     | Saturation-excess Runoff, Sbsrf   | [m³/s] |
        +-------+-----------------------------------+--------+
        | 4     | Perched Return Flow, Psrf         | [m³/s] |
        +-------+-----------------------------------+--------+
        | 5     | Groundwater Exfiltration, Satsrf  | [m³/s] |
        +-------+-----------------------------------+--------+

Spatial Output
----------------

Dynamic Spatial Output Tables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  **Table 6.9** Content of *timestamp_00d files

        .. tabularcolumns:: |c|c|c|

        +-------+---------------------------------------+----------+
        | Column| Description                           | Units    |
        +=======+=======================================+==========+
        | 1     | Node Identification, ID               | [id]     |
        +-------+---------------------------------------+----------+
        | 2     | Depth to groundwater table, Nwt       | [mm]     |
        +-------+---------------------------------------+----------+
        | 3     | Total moisture above the water table, | [mm]     |
        |       | Mu                                    |          |
        +-------+---------------------------------------+----------+
        | 4     | Moisture content in the initialization| [mm]     |
        |       | profile, Mi                           |          |
        +-------+---------------------------------------+----------+
        | 5     | Wetting front depth, Nf               | [mm]     |
        +-------+---------------------------------------+----------+
        | 6     | Top front depth, Nt                   | [mm]     |
        +-------+---------------------------------------+----------+
        | 7     | Unsaturated lateral flow out from     | [mm/hr]  |
        |       | cell, Qpout                           |          |
        +-------+---------------------------------------+----------+
        | 8     | Unsaturated lateral flow into cell,   | [mm/hr]  |
        |       | Qpin                                  |          |
        +-------+---------------------------------------+----------+
        | 9     | Surface Runoff, Srf                   | [mm]     |
        +-------+---------------------------------------+----------+
        | 10    | Rainfall, Rain                        | [mm/hr]  |
        +-------+---------------------------------------+----------+
        | 11    | Snow Temperature, ST                  | [°C]     |
        +-------+---------------------------------------+----------+
        | 12    | Ice Part of Snow Water Equivalent, IWE| [cm]     |
        +-------+---------------------------------------+----------+
        | 13    | Liquid Part of Snow Water Equivalent, | [cm]     |
        |       | LWE                                   |          | 
        +-------+---------------------------------------+----------+
        | 14    | Snow Sublimation, SnSu                | [cm]     |
        +-------+---------------------------------------+----------+
        | 15    | Snow Evaporation, SnEvap              | [cm]     |
        +-------+---------------------------------------+----------+
        | 16    | Snow Melt, SnMelt                     | [cm]     |
        +-------+---------------------------------------+----------+
        | 17    | Internal Energy of Snow Pack, Upack   | [kJ/m²]  |
        +-------+---------------------------------------+----------+
        | 18    | Latent Heat Flux from Snow Cover, sLHF| [kJ/m²]  |
        +-------+---------------------------------------+----------+
        | 19    | Sensible Heat Flux from Snow Cover,   | [kJ/m²]  |
        |       | sSHF                                  |          |
        +-------+---------------------------------------+----------+
        | 20    | Ground Heat Flux from Snow Cover, sGHF| [kJ/m²]  |
        +-------+---------------------------------------+----------+
        | 21    | Precipitation Heat Flux from Snow     | [kJ/m²]  |
        |       | Cover, sPHF                           |          |
        +-------+---------------------------------------+----------+
        | 22    | Outgoing Longwave Radiation from Snow | [kJ/m²]  |
        |       | Cover, sRLo                           |          |
        +-------+---------------------------------------+----------+
        | 23    | Incoming Longwave Radation from Snow  | [kJ/m²]  |
        |       | Cover, sRLi                           |          |
        +-------+---------------------------------------+----------+
        | 24    | Incoming Shortwave Radiation from Snow| [kJ/m²]  |
        |       | Cover, sRSi                           |          |
        +-------+---------------------------------------+----------+
        | 25    | Error in Energy Balance, Uerr         | [J/m²]   |
        +-------+---------------------------------------+----------+
        | 26    | Intercepted SWE, IntSWE               | [cm]     |
        +-------+---------------------------------------+----------+
        | 27    | Sublimated Snow from Canopy, IntSub   | [cm]     |
        +-------+---------------------------------------+----------+
        | 28    | Unloaded Snow from Canopy, IntUnl     | [cm]     |
        +-------+---------------------------------------+----------+
        | 29    | Soil Moisture, top 10 cm, SoilMoist   | [ ]      |
        +-------+---------------------------------------+----------+
        | 30    | Root Zone Moisture, top 1 m, RootMoist| [ ]      |
        +-------+---------------------------------------+----------+
        | 31    | Canopy Storage, CanStorage            | [mm]     |
        +-------+---------------------------------------+----------+
        | 32    | Actual Evaporation, ActEvp            | [mm/hr]  |
        +-------+---------------------------------------+----------+
        | 33    | Evaporation from Bare Soil, EvpSoil   | [mm/hr]  |
        +-------+---------------------------------------+----------+
        | 34    | Total Evapotranspiration, ET          | [mm/hr]  |
        +-------+---------------------------------------+----------+
        | 35    | Ground Heat Flux, Gflux               | [W/m²]   |
        +-------+---------------------------------------+----------+
        | 36    | Sensible Heat Flux, Hflux             | [W/m²]   |
        +-------+---------------------------------------+----------+
        | 37    | Latent Heat Flux, Lflux               | [W/m²]   |
        +-------+---------------------------------------+----------+
        | 38    | Discharge, Qstrm                      | [m³/s]   |
        +-------+---------------------------------------+----------+
        | 39    | Channel Stage, Hlev                   | [m]      |
        +-------+---------------------------------------+----------+
        | 40    | Channel Flow Velocity, FlwVlc         | [m/s]    |
        +-------+---------------------------------------+----------+
        | 41    | Canopy Storage Parameter, CanStorParam| [mm]     |
        +-------+---------------------------------------+----------+
        | 42    | Interception Coeff., IntercepCoeff.   | [ ]      |
        +-------+---------------------------------------+----------+
        | 43    | Free Throughfall Coeff.- Rutter,      | [ ]      |
        |       | ThroughFall                           |          |
        +-------+---------------------------------------+----------+
        | 44    | Canopy Field Capacity – Rutter,       | [mm]     |
        |       | CanFieldCap                           |          |
        +-------+---------------------------------------+----------+
        | 45    | Drainage coefficient – Rutter,        | [mm/hr]  |
        |       | DrainCoeff                            |          |
        +-------+---------------------------------------+----------+
        | 46    | Drainage Expon. Param. – Rutter,      | [mm⁻¹]   |
        |       | DrainExpPar                           |          |
        +-------+---------------------------------------+----------+
        | 47    | Albedo, LandUseAlb                    | [ ]      |
        +-------+---------------------------------------+----------+
        | 48    | Vegetation Height , VegHeight         | [m]      |
        +-------+---------------------------------------+----------+
        | 49    | Optical Transmission Coeff.,          | [ ]      |
        |       | OptTransmCoeff                        |          |
        +-------+---------------------------------------+----------+
        | 50    | Canopy- Average Stomatal Resistance,  | [s/m]    |
        |       | StomRes                               |          |
        +-------+---------------------------------------+----------+
        | 51    | Vegetation Fraction, VegFraction      | [ ]      |
        +-------+---------------------------------------+----------+
        | 52    | Canopy Leaf Area Index, LeafAI        | [ ]      |
        +-------+---------------------------------------+----------+


Time-integrated Spatial Output Table
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  **Table 6.10** Content of *timestamp_00i file

        .. tabularcolumns:: |c|c|c|

        +-------+----------------------------------------+-------------+
        | Column| Description                            | Units       |
        +=======+========================================+=============+
        | 1     | Node Identification, ID                | [id]        |
        +-------+----------------------------------------+-------------+
        | 2     | Boundary Flag, BndCd                   | [ ]         |
        +-------+----------------------------------------+-------------+
        | 3     | Elevation, Z                           | [m]         |
        +-------+----------------------------------------+-------------+
        | 4     | Voronoi Area, VAr                      | [m²]        |
        +-------+----------------------------------------+-------------+
        | 5     | Contributing Area, CAr                 | [km²]       |
        +-------+----------------------------------------+-------------+
        | 6     | Curvature, Curv                        | [ ]         |
        +-------+----------------------------------------+-------------+
        | 7     | Flow Edge Length, EdgL                 | [m]         |
        +-------+----------------------------------------+-------------+
        | 8     | Tangent of Flow Edge Slope, tan(Slp)   | [ ]         |
        +-------+----------------------------------------+-------------+
        | 9     | Width of Voronoi Flow Window, FWidth   | [m]         |
        +-------+----------------------------------------+-------------+
        | 10    | Site Aspect as Angle from North, Aspect| [radian]    |
        +-------+----------------------------------------+-------------+
        | 11    | Sky View Factor, SV                    | [ ]         |
        +-------+----------------------------------------+-------------+
        | 12    | Land View Factor, LV                   | [ ]         |
        +-------+----------------------------------------+-------------+
        | 13    | Average Soil Moisture, top 10 cm, AvSM | [ ]         |
        +-------+----------------------------------------+-------------+
        | 14    | Average Root Zone Moisture, top 1 m,   | [ ]         |
        |       | AvRtM                                  |             |
        +-------+----------------------------------------+-------------+
        | 15    | Infiltration-excess Runoff Occurences, | [# of       |
        |       | HOccr                                  | TIMESTEP]   |
        +-------+----------------------------------------+-------------+
        | 16    | Infiltration-excess Runoff Average     | [mm/hr]     |
        |       | Rate, HRt                              |             |
        +-------+----------------------------------------+-------------+
        | 17    | Saturation-excess Runoff Occurences,   | [# of       |
        |       | SbOccr                                 | TIMESTEP]   |
        +-------+----------------------------------------+-------------+
        | 18    | Saturation-excess Runoff Average Rate, | [mm/hr]     |
        |       | SbRt                                   |             |
        +-------+----------------------------------------+-------------+
        | 19    | Perched Return Runoff Occurences,      | [# of       |
        |       | POccr                                  | TIMESTEP]   |
        +-------+----------------------------------------+-------------+
        | 20    | Perched Return Runoff Average Rate,    | [mm/hr]     |
        |       | PRt                                    |             |
        +-------+----------------------------------------+-------------+
        | 21    | Groundwater Exfiltration Runoff        | [# of       |
        |       | Occurences, SatOccr                    | GWSTEP]     |
        +-------+----------------------------------------+-------------+
        | 22    | Groundwater Exfiltration Runoff        | [mm/hr]     |
        |       | Average Rate, SatRt                    |             |
        +-------+----------------------------------------+-------------+
        | 23    | Soil Saturation Occurences, SoiSatOccr | [# of       |
        |       |                                        | TIMESTEP]   |
        +-------+----------------------------------------+-------------+
        | 24    | Recharge-Discharge Variable, RchDsch   | [m]         |
        +-------+----------------------------------------+-------------+
        | 25    | Average Evapotranspiration, AveET      | [mm/hr]     |
        +-------+----------------------------------------+-------------+
        | 26    | Evaporative Fraction, EvpFrct          | [ ]         |
        +-------+----------------------------------------+-------------+
        | 27    | Cumulative Evapotranspiration, cET     | [mm]        |
        +-------+----------------------------------------+-------------+
        | 28    | Cumulative Soil Evaporation, cEsoil    | [mm]        |
        +-------+----------------------------------------+-------------+
        | 29    | Cumulative Latent Heat Flux from Snow  | [kJ/m²]     |
        |       | Cover, cLHF                            |             |
        +-------+----------------------------------------+-------------+
        | 30    | Cumulative Melt, cMelt                 | [cm]        |
        +-------+----------------------------------------+-------------+
        | 31    | Cumulative Sensible Heat Flux from     |  [kJ/m²]    |
        |       | Snow Cover, cSHF                       |             |
        +-------+----------------------------------------+-------------+
        | 32    | Cumulative Precipitation Heat Flux     | [kJ/m²]     |
        |       | from Snow Cover, cPHF                  |             |
        +-------+----------------------------------------+-------------+
        | 33    | Cumulative Incoming Longwave           | [kJ/m²]     |
        |       | Radiation from Snow Cover, cRLIn       |             |
        +-------+----------------------------------------+-------------+
        | 34    | Cumulative Outgoing Longwave           | [kJ/m²]     |
        |       | Radiation from Snow Cover, cRLo        |             |
        +-------+----------------------------------------+-------------+
        | 35    | Cumulative Incoming Shortwave          | [kJ/m²]     |
        |       | Radiation from Snow Cover, cRSIn       |             |
        +-------+----------------------------------------+-------------+
        | 36    | Cumulative Ground Heat Flux from       | [kJ/m²]     |
        |       | Snow Cover, cGHF                       |             |
        +-------+----------------------------------------+-------------+
        | 37    | Cumulative Energy Balance Error, cUErr | [kJ/m²]     |
        +-------+----------------------------------------+-------------+
        | 38    | Cumulative Hrs of Sun exposure,cHrsSun | [hr]        |
        +-------+----------------------------------------+-------------+
        | 39    | Cumulative Hours Snow Covered, cHrsSnow| [hr]        |
        +-------+----------------------------------------+-------------+
        | 40    | Longest Time of Continuous Snow        | [hr]        |
        |       | Cover, persTime                        |             |
        +-------+----------------------------------------+-------------+
        | 41    | Maximum Season SWE, peakWE             | [cm]        |
        +-------+----------------------------------------+-------------+
        | 42    | Simulation Hour of Maximum SWE,        | [hr]        |
        |       | peakTime                               |             |
        +-------+----------------------------------------+-------------+
        | 43    | Simulation Hr of Initial SWE, initTime | [hr]        |
        +-------+----------------------------------------+-------------+
        | 44    | Cumulative Sublimated Snow from        | [cm]        |
        |       | Canopy, cIntSub                                      |
        +-------+----------------------------------------+-------------+
        | 45    | Cumulative Sublimaton from Snow Pack,  |  [cm]       |  
        |       | cSnSub                                 |             |
        +-------+----------------------------------------+-------------+
        | 46    | Cumulative Evaporation from Snow Pack, | [cm]        |
        |       | cSnEvap                                |             | 
        +-------+----------------------------------------+-------------+
        | 47    | Cumulative Unloaded Snow from Canopy,  | [cm]        |
        |       | cIntUnl                                |             |
        +-------+----------------------------------------+-------------+
        | 48    | Av. Canopy Storage Parameter,          | [mm]        |
        |       | AvCanStorParam                         |             |
        +-------+----------------------------------------+-------------+
        | 49    | Av. Intercep. Coeff., AvIntercCoeff    | [ ]         |
        +-------+----------------------------------------+-------------+
        | 50    | Av. Free Throughfall Coeff.- Rutter,   | [ ]         |
        |       | AvTF                                   |             |
        +-------+----------------------------------------+-------------+
        | 51    | Av. Canopy Field Capac. – Rutter,      | [mm]        |
        |       | AvCanFieldCap                          |             |
        +-------+----------------------------------------+-------------+
        | 52    | Av. Drain. Coeff. – Rutter,            | [mm/hr]     |
        |       | AvDrainCoeff                           |             |
        +-------+----------------------------------------+-------------+
        | 53    | Av. Drain. Expon. Param. – Rutter,     | [mm⁻¹]      |
        |       | AvDrainExpPar                          |             |
        +-------+----------------------------------------+-------------+
        | 54    | Av. Albedo,AvLUAlb                     | [ ]         |
        +-------+----------------------------------------+-------------+
        | 55    | Av. Veg. Height , AvVegHeight          | [m]         |
        +-------+----------------------------------------+-------------+
        | 56    | Av. Optical Transm. Coeff., AvOTCoeff  | [ ]         |
        +-------+----------------------------------------+-------------+
        | 57    | Av. Canopy- Average Stom. Resist.,     | [s/m]       |
        |       | AvStomRes                              |             |
        +-------+----------------------------------------+-------------+
        | 58    | Av. Veg. Frac., AvVegFract             | [ ]         |
        +-------+----------------------------------------+-------------+
        | 59    | Av. Canopy Leaf Area Index, AvLeafAI   | [ ]         |
        +-------+----------------------------------------+-------------+
        | 60    | Depth to Bedrock, Bedrock_Depth_mm     | [mm]        |
        +-------+----------------------------------------+-------------+
        | 61    | Saturate Hydraulic Conducitivity, Ks   | [mm/hr]     |
        +-------+----------------------------------------+-------------+
        | 62    | Saturated Soil Moisture, ThetaS        | [-]         |
        +-------+----------------------------------------+-------------+
        | 63    | Residual Soil Moisture, ThetaR         | [-]         |
        +-------+----------------------------------------+-------------+
        | 64    | Pore Distribution Index, PoreSize      | [-]         |
        +-------+----------------------------------------+-------------+
        | 65    | Air Entry Bubbling Pressure,           |[mm]         |
        |       | AirEBubP                               |(negative)   |     
        +-------+----------------------------------------+-------------+
        | 66    | Hydraulic Decay Parameter, DecayF      | [1/mm]      |
        +-------+----------------------------------------+-------------+
        | 67    | Saturated Anisotropy Ratio, SatAnRatio | [-]         |
        +-------+----------------------------------------+-------------+
        | 68    | Unsaturated Anisotropy Ratio,          | [-]         |
        |       | UnsatAnRatio                           |             |
        +-------+----------------------------------------+-------------+
        | 69    | Porosity, Porosity                     | [-]         |
        +-------+----------------------------------------+-------------+
        | 70    | Volumetric Heat Conductivity,          | [J/msK]     |
        |       | VolHeatCond                            |             |
        +-------+----------------------------------------+-------------+
        | 71    | Soil Heat Capacity, SoilHeatCap        | [J/m^k]     |
        +-------+----------------------------------------+-------------+
        | 72    | Soil Class, SoilID                     | [-]         |
        +-------+----------------------------------------+-------------+ 
        | 73    | Landuse Class, LandUseID               | [-]         |
        +-------+----------------------------------------+-------------+   






