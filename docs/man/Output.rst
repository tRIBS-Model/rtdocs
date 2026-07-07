Model Outputs
==================================

The tRIBS Model produces a number of output files that represent the time series or the spatial distribution of model state or output variables. Output variables include the position of moisture fronts in the unsaturated zone, water table elevation, surface runoff, subsurface flux, rainfall rate, interception loss, evapotranspiration, and information on the mesh triangulation. **Table 6.1**, **Table 6.2**, and **Table 6.3** summarize: (1) mesh output files, (2) time series outputs, and (3) spatial outputs. More detailed descriptions of the individual files are provided in the following sections.

With the exception of the mesh output files (**Table 6.1**), all output files described on this page are CSV with a single header row; each column header combines the variable name and its units.

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

    The location of the output files is specified in the tRIBS Model Input File using the keyword *OUTFILENAME*, which serves as the single base pathname for the spatial, hydrologic and outlet output. An important note to make is that the ``*.mrf``, ``*.rft`` and ``*.dat`` files produced by the model are labeled with additional identifiers before the extension that relate to the time of the output. For each *OPINTRVL* time step, the model will produce output of the ``*.mrf`` type, while the ``*.rft`` file is produced only after completion of the entire run. The spatial output (``*timestamp_00d``) are determined by the time step specified in the *SPOPINTRVL* keyword. Time-integrated spatial output (``*timestamp_00i``) is produced only at the end of the simulation. The model also produces various files with a ``*.pixel`` extension. The ``*.pixel`` files contain the dynamic variable output for a single node for all model times. The nodes for which ``*.pixel`` files are produced are specified through a Node Output List (``*.nol``) File, described below; the same file structure is used for the *OUTLETNODELIST* keyword to request interior ``*.qout`` streamflow output at specific nodes.

    **Table 6.4** Node/Outlet Output List File Structure (``*.nol``)

    Requested locations can be specified either by node ID or by coordinate; which mode applies is determined by the header row. ID-based (header ``ID``):

            .. tabularcolumns:: |c|

            +--------+
            | *ID*   |
            +--------+
            | 105    |
            +--------+
            | 250    |
            +--------+
            | 407    |
            +--------+

    Coordinate-based (header ``X,Y``):

            .. tabularcolumns:: |c|c|

            +-------------+-------------+
            | *X*         | *Y*         |
            +-------------+-------------+
            | 456000.0    | 3688978.5   |
            +-------------+-------------+
            | 457500.0    | 3689200.0   |
            +-------------+-------------+

    Both files are CSV with a single header-flag first line and no leading count line; every row after the header is read. Coordinates must be given in the same x/y projection as the mesh, not latitude/longitude, and are resolved at read time to the nearest eligible mesh node. *NODEOUTPUTLIST* resolves to the nearest active computational node, while *OUTLETNODELIST* is restricted to the channel network. The resolved node ID is then used for file naming like in the ID-based path, so the contents and column layout of ``*.pixel`` and ``*.qout`` are unchanged; only how the locations are requested has changed. The resolved snap distance is printed as a diagnostic for every coordinate, and the model warns (without stopping the run) if a coordinate falls outside the domain, or, for outlet requests, if the nearest stream node is much farther away than the nearest node of any type, a sign the coordinate isn't actually on the channel.

Time Series
-----------

Basin Outlet Discharge Time Series
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  **Table 6.5** Content of *_Outlet.qout file, or *.qout file for interior nodes requested via OUTLETNODELIST

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

        .. tabularcolumns:: |c|c|c|c|

        +--------+----------------------+----------------------------------------------------+---------+
        | Column | Variable             | Description                                        | Units   |
        +--------+----------------------+----------------------------------------------------+---------+
        | 1      | NodeID               | Node Identification                                | [id]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 2      | Time_hr              | Time                                               | [hr]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 3      | Nwt_mm               | Depth to groundwater table                         | [mm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 4      | Nf_mm                | Wetting front depth                                | [mm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 5      | Nt_mm                | Top front depth                                    | [mm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 6      | Mu_mm                | Total moisture above the water table               | [mm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 7      | Mi_mm                | Moisture content in the initialization profile     | [mm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 8      | QpOut_mm_h           | Unsaturated lateral flow out from cell             | [mm/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 9      | QpIn_mm_h            | Unsaturated lateral flow into cell                 | [mm/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 10     | Trnsm_m2_h           | Transmissivity                                     | [m2/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 11     | GWflx_m3_h           | Groundwater flux                                   | [m3/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 12     | Srf_Hour_mm          | Surface Runoff (hourly)                            | [mm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 13     | Rain_mm_h            | Rainfall                                           | [mm/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 14     | SoilMoist_[]         | Soil Moisture, top 10 cm                           | [-]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 15     | RootMoist_[]         | Root Zone Moisture, top 1 m                        | [-]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 16     | AirT_oC              | Air Temperature                                    | [C]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 17     | DewT_oC              | Dew Point Temperature                              | [C]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 18     | SurfT_oC             | Surface Temperature                                | [C]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 19     | SoilT_oC             | Soil Temperature                                   | [C]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 20     | Press_Pa             | Atmospheric Pressure                               | [Pa]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 21     | RelHum_[]            | Relative Humidity                                  | [-]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 22     | SkyCov_[]            | Sky Cover                                          | [-]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 23     | Wind_m_s             | Wind Speed                                         | [m/s]   |
        +--------+----------------------+----------------------------------------------------+---------+
        | 24     | NetRad_W_m2          | Net Radiation                                      | [W/m2]  |
        +--------+----------------------+----------------------------------------------------+---------+
        | 25     | ShrtRadIn_W_m2       | Incoming Shortwave Radiation                       | [W/m2]  |
        +--------+----------------------+----------------------------------------------------+---------+
        | 26     | ShortRadInSlope_W_m2 | Incoming Shortwave Radiation to the Sloped Surface | [W/m2]  |
        +--------+----------------------+----------------------------------------------------+---------+
        | 27     | ShrtRadIn_dir_W_m2   | Incoming Direct Shortwave Radiation                | [W/m2]  |
        +--------+----------------------+----------------------------------------------------+---------+
        | 28     | ShrtRadIn_dif_W_m2   | Incoming Diffuse Shortwave Radiation               | [W/m2]  |
        +--------+----------------------+----------------------------------------------------+---------+
        | 29     | ShortAbsbVeg_W_m2    | Shortwave Absorbed Radiation, Vegetation           | [W/m2]  |
        +--------+----------------------+----------------------------------------------------+---------+
        | 30     | ShortAbsbSoi_W_m2    | Shortwave Absorbed Radiation, Soil                 | [W/m2]  |
        +--------+----------------------+----------------------------------------------------+---------+
        | 31     | LngRadIn_W_m2        | Incoming Longwave Radiation                        | [W/m2]  |
        +--------+----------------------+----------------------------------------------------+---------+
        | 32     | LngRadOut_W_m2       | Outgoing Longwave Radiation                        | [W/m2]  |
        +--------+----------------------+----------------------------------------------------+---------+
        | 33     | PotEvp_mm_h          | Potential Evaporation                              | [mm/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 34     | ActEvp_mm_h          | Actual Evaporation                                 | [mm/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 35     | EvpTtrs_mm_h         | Total Evapotranspiration                           | [mm/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 36     | EvpWetCan_mm_h       | Evaporation from Wet Canopy                        | [mm/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 37     | EvpDryCan_mm_h       | Evaporation from Dry Canopy                        | [mm/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 38     | EvpSoil_mm_h         | Evaporation from Bare Soil                         | [mm/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 39     | Gflux_W_m2           | Ground Heat Flux                                   | [W/m2]  |
        +--------+----------------------+----------------------------------------------------+---------+
        | 40     | HFlux_W_m2           | Sensible Heat Flux                                 | [W/m2]  |
        +--------+----------------------+----------------------------------------------------+---------+
        | 41     | Lflux_W_m2           | Latent Heat Flux                                   | [W/m2]  |
        +--------+----------------------+----------------------------------------------------+---------+
        | 42     | NetPrecip_mm_hr      | Net Precipitation                                  | [mm/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 43     | LiqWE_cm             | Liquid Water Equivalent                            | [cm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 44     | IceWE_cm             | Ice Water Equivalent                               | [cm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 45     | SnWE_cm              | Snow Water Equivalent                              | [cm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 46     | SnSub_cm             | Sublimation from Snowpack                          | [cm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 47     | SnEvap_cm            | Evaporation from Snowpack                          | [cm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 48     | U_kJ_m2              | Internal Energy of Snow Pack                       | [kJ/m2] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 49     | RouteWE_cm           | Routed Melt Water Equivalent                       | [cm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 50     | SnTemp_C             | Snow Temperature                                   | [C]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 51     | SurfAge_h            | Snow Surface Age                                   | [hr]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 52     | SnDepth_cm           | Snow Depth                                         | [cm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 53     | SnDensity_kg_m3      | Snow Density                                       | [kg/m3] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 54     | DU_kJ_m2             | Change in Snow Pack Internal Energy                | [kJ/m2] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 55     | snLHF_kJ_m2          | Latent Heat Flux from Snow Cover                   | [kJ/m2] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 56     | snSHF_kJ_m2          | Sensible Heat Flux from Snow Cover                 | [kJ/m2] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 57     | snGHF_kJ_m2          | Ground Heat Flux from Snow Cover                   | [kJ/m2] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 58     | snPHF_kJ_m2          | Precip Heat Flux from Snow Cover                   | [kJ/m2] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 59     | snRLout_kJ_m2        | Outgoing Longwave Radiation from Snow              | [kJ/m2] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 60     | snRLin_kJ_m2         | Incoming Longwave Radiation from Snow              | [kJ/m2] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 61     | snRSin_kJ_m2         | Incoming Shortwave Radiation from Snow             | [kJ/m2] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 62     | Uerror_kJ_m2         | Error in Energy Balance                            | [kJ/m2] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 63     | IntSWEq_cm           | Intercepted Snow Water Equivalent                  | [cm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 64     | IntSub_cm            | Sublimated Snow Water Equivalent from Canopy       | [cm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 65     | IntSnUnload_cm       | Unloaded Snow Water Equivalent from Canopy         | [cm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 66     | CanStorage_mm        | Canopy Storage                                     | [mm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 67     | CumIntercept_mm      | Cumulative Interception                            | [mm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 68     | Interception_mm      | Interception                                       | [mm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 69     | Recharge_mm/hr       | Recharge                                           | [mm/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 70     | RunOn_mm             | Runon                                              | [mm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 71     | Qstrm_m3_s           | Discharge                                          | [m3/s]  |
        +--------+----------------------+----------------------------------------------------+---------+
        | 72     | Hlevel_m             | Channel Stage                                      | [m]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 73     | ThroughFall_[]       | Free Throughfall Coefficient - Rutter              | [-]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 74     | CanFieldCap_mm       | Canopy Field Capacity - Rutter                     | [mm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 75     | DrainCoeff_mm_hr     | Drainage Coefficient - Rutter                      | [mm/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 76     | DrainExpPar_1_mm     | Drainage Exponent Parameter - Rutter               | [mm-1]  |
        +--------+----------------------+----------------------------------------------------+---------+
        | 77     | LandUseAlb_[]        | Albedo                                             | [-]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 78     | VegHeight_m          | Vegetation Height                                  | [m]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 79     | OptTransmCoeff_[]    | Optical Transmission Coefficient                   | [-]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 80     | StomRes_s_m          | Canopy-Average Stomatal Resistance                 | [s/m]   |
        +--------+----------------------+----------------------------------------------------+---------+
        | 81     | VegFraction_[]       | Vegetation Fraction                                | [-]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 82     | LeafAI_[]            | Canopy Leaf Area Index                             | [-]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 83     | RootZoneDepth_m      | Rootzone Depth                                     | [m]     |
        +--------+----------------------+----------------------------------------------------+---------+

Basin-averaged Hydrological Time Series
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  **Table 6.7** Content of *.mrf file

        .. tabularcolumns:: |c|c|c|c|

        +--------+----------------+--------------------------------------------------------------+---------+
        | Column | Variable       | Description                                                  | Units   |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 1      | Time_hr        | Time                                                         | [hr]    |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 2      | Srf_m3_s       | Surface Runoff from Hydrologic Routing                       | [m3/s]  |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 3      | MAP_mm_hr      | Mean Areal Precipitation                                     | [mm/hr] |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 4      | RainMax_mm_hr  | Maximum Rainfall Rate                                        | [mm/hr] |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 5      | RainMin_mm_hr  | Minimum Rainfall Rate                                        | [mm/hr] |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 6      | MSM100_[]      | Mean Surface Soil Moisture, top 10 cm                        | [-]     |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 7      | MSMRt_[]       | Mean Soil Moisture in Root Zone, top 1 m                     | [-]     |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 8      | MSMU_[]        | Mean Soil Moisture in Unsaturated Zone (above water table)   | [-]     |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 9      | MDGW_mm        | Mean Depth to Groundwater                                    | [mm]    |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 10     | MET_mm         | Mean Evapotranspiration                                      | [mm]    |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 11     | SatPercent_[]  | Areal Fraction of Surface Saturation                         | [-]     |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 12     | RainPercent_[] | Areal Fraction of Rainfall                                   | [-]     |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 13     | AvSWE_cm       | Average Snow Water Equivalent                                | [cm]    |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 14     | AvMelt_cm      | Average Amount of Snow Melt                                  | [cm]    |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 15     | AvSnSub_cm     | Average Sublimation from Snowpack                            | [cm]    |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 16     | AvSnEvap_cm    | Average Evaporation from Snowpack                            | [cm]    |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 17     | AvSTC_C        | Average Snow Temperature                                     | [C]     |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 18     | AvDUInt_kJ_m2  | Average Change in Snow Pack Internal Energy                  | [kJ/m2] |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 19     | AvSLHF_kJ_m2   | Average Latent Heat Flux from Snow Covered Areas             | [kJ/m2] |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 20     | AvSSHF_kJ_m2   | Average Sensible Heat Flux from Snow Covered Areas           | [kJ/m2] |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 21     | AvSPHF_kJ_m2   | Average Precipitation Heat Flux from Snow Covered Areas      | [kJ/m2] |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 22     | AvSGHF_kJ_m2   | Average Ground Heat Flux from Snow Covered Areas             | [kJ/m2] |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 23     | AvSRLI_kJ_m2   | Average Incoming Longwave Radiation from Snow Covered Areas  | [kJ/m2] |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 24     | AvSRLO_kJ_m2   | Average Outgoing Longwave Radiation from Snow Covered Areas  | [kJ/m2] |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 25     | AvSRSI_kJ_m2   | Average Incoming Shortwave Radiation from Snow Covered Areas | [kJ/m2] |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 26     | AvInSn_cm      | Mean Intercepted Snow Water Equivalent                       | [cm]    |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 27     | AvInSu_cm      | Mean Sublimation from Intercepted Snow                       | [cm]    |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 28     | AvInUn_cm      | Mean Unloaded Snow from Canopy                               | [cm]    |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 29     | SCA_[]         | Fraction Snow Covered Area                                   | [-]     |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 30     | ChannelPerc_m3 | Channel Percolation                                          | [m3]    |
        +--------+----------------+--------------------------------------------------------------+---------+
        | 31     | Qunsat_mm_hr   | Net Outflow from Unsaturated Zone                            | [mm/hr] |
        +--------+----------------+--------------------------------------------------------------+---------+

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






