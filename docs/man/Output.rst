Model Outputs
==================================

The tRIBS Model produces a number of output files that represent the time series or the spatial distribution of model state or output variables. Output variables include the position of moisture fronts in the unsaturated zone, water table elevation, surface runoff, subsurface flux, rainfall rate, interception loss, evapotranspiration, and information on the mesh triangulation. **Table 8.1**, **Table 8.2**, and **Table 8.3** summarize: (1) mesh output files, (2) time series outputs, and (3) spatial outputs. More detailed descriptions of the individual files are provided in the following sections.

With the exception of the mesh output files (**Table 8.1**), all output files described on this page are CSV with a single header row; each column header combines the variable name and its units.

    **Table 8.1** tRIBS Mesh Output Files

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

    **Table 8.2** tRIBS Model Time Series Files

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

    **Table 8.3** tRIBS Model Spatial Output Files

            .. tabularcolumns::  |c|c|l|

            +------------------------------+------------------+----------------------------------------------------------------+
            |Model Spatial Output Files    |  Extension       |  Description                                                   |
            +==============================+==================+================================================================+
            |*Mesh Dynamic Output File*    |``*timestamp_00d``|  Dynamic variable output for all mesh nodes at specific time.  |
            +------------------------------+------------------+----------------------------------------------------------------+
            |*Mesh Integrated Output File* |``*timestamp_00i``|  Time-integrated variable output for all mesh nodes.           |
            +------------------------------+------------------+----------------------------------------------------------------+

    The location of the output files is specified in the tRIBS Model Input File using the keyword *OUTFILENAME*, which serves as the single base pathname for the spatial, hydrologic and outlet output. An important note to make is that the ``*.mrf``, ``*.rft`` and ``*.dat`` files produced by the model are labeled with additional identifiers before the extension that relate to the time of the output. For each *OPINTRVL* time step, the model will produce output of the ``*.mrf`` type, while the ``*.rft`` file is produced only after completion of the entire run. The spatial output (``*timestamp_00d``) are determined by the time step specified in the *SPOPINTRVL* keyword. Time-integrated spatial output (``*timestamp_00i``) is produced only at the end of the simulation. The model also produces various files with a ``*.pixel`` extension. The ``*.pixel`` files contain the dynamic variable output for a single node for all model times. The nodes for which ``*.pixel`` files are produced are specified through a Node Output List (``*.nol``) File, described below; the same file structure is used for the *OUTLETNODELIST* keyword to request interior ``*.qout`` streamflow output at specific nodes.

    **Table 8.4** Node/Outlet Output List File Structure (``*.nol``)

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

  **Table 8.5** Content of *_Outlet.qout file, or *.qout file for interior nodes requested via OUTLETNODELIST

        .. tabularcolumns:: |c|c|c|c|

        +--------+------------+---------------+--------+
        | Column | Variable   | Description   | Units  |
        +--------+------------+---------------+--------+
        | 1      | Time_hr    | Time          | [hr]   |
        +--------+------------+---------------+--------+
        | 2      | Qstrm_m3_s | Discharge     | [m3/s] |
        +--------+------------+---------------+--------+
        | 3      | Hlev_m     | Channel Stage | [m]    |
        +--------+------------+---------------+--------+


Hydrologic Time Series at Selected TIN nodes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  **Table 8.6** Content of *.pixel files

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
        | 34     | EvpTtrs_mm_h         | Total Evapotranspiration                           | [mm/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 35     | EvpWetCan_mm_h       | Evaporation from Wet Canopy                        | [mm/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 36     | TransDryCan_mm_h     | Evaporation from Dry Canopy                        | [mm/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 37     | EvpSoil_mm_h         | Evaporation from Bare Soil                         | [mm/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 38     | Gflux_W_m2           | Ground Heat Flux                                   | [W/m2]  |
        +--------+----------------------+----------------------------------------------------+---------+
        | 39     | HFlux_W_m2           | Sensible Heat Flux                                 | [W/m2]  |
        +--------+----------------------+----------------------------------------------------+---------+
        | 40     | Lflux_W_m2           | Latent Heat Flux                                   | [W/m2]  |
        +--------+----------------------+----------------------------------------------------+---------+
        | 41     | NetPrecip_mm_hr      | Net Precipitation                                  | [mm/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 42     | LiqWE_cm             | Liquid Water Equivalent                            | [cm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 43     | IceWE_cm             | Ice Water Equivalent                               | [cm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 44     | SnWE_cm              | Snow Water Equivalent                              | [cm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 45     | SnSub_cm             | Sublimation from Snowpack                          | [cm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 46     | SnEvap_cm            | Evaporation from Snowpack                          | [cm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 47     | U_kJ_m2              | Internal Energy of Snow Pack                       | [kJ/m2] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 48     | RouteWE_cm           | Routed Melt Water Equivalent                       | [cm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 49     | SnTemp_C             | Snow Temperature                                   | [C]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 50     | SurfAge_h            | Snow Surface Age                                   | [hr]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 51     | SnDepth_cm           | Snow Depth                                         | [cm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 52     | SnDensity_kg_m3      | Snow Density                                       | [kg/m3] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 53     | DU_kJ_m2             | Change in Snow Pack Internal Energy                | [kJ/m2] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 54     | snLHF_kJ_m2          | Latent Heat Flux from Snow Cover                   | [kJ/m2] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 55     | snSHF_kJ_m2          | Sensible Heat Flux from Snow Cover                 | [kJ/m2] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 56     | snGHF_kJ_m2          | Ground Heat Flux from Snow Cover                   | [kJ/m2] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 57     | snPHF_kJ_m2          | Precip Heat Flux from Snow Cover                   | [kJ/m2] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 58     | snRLout_kJ_m2        | Outgoing Longwave Radiation from Snow              | [kJ/m2] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 59     | snRLin_kJ_m2         | Incoming Longwave Radiation from Snow              | [kJ/m2] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 60     | snRSin_kJ_m2         | Incoming Shortwave Radiation from Snow             | [kJ/m2] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 61     | Uerror_kJ_m2         | Error in Energy Balance                            | [kJ/m2] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 62     | IntSWEq_cm           | Intercepted Snow Water Equivalent                  | [cm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 63     | IntSub_cm            | Sublimated Snow Water Equivalent from Canopy       | [cm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 64     | IntSnUnload_cm       | Unloaded Snow Water Equivalent from Canopy         | [cm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 65     | CanStorage_mm        | Canopy Storage                                     | [mm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 66     | CumIntercept_mm      | Cumulative Interception                            | [mm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 67     | Interception_mm      | Interception                                       | [mm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 68     | Recharge_mm/hr       | Recharge                                           | [mm/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 69     | RunOn_mm             | Runon                                              | [mm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 70     | Qstrm_m3_s           | Discharge                                          | [m3/s]  |
        +--------+----------------------+----------------------------------------------------+---------+
        | 71     | Hlevel_m             | Channel Stage                                      | [m]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 72     | ThroughFall_[]       | Free Throughfall Coefficient - Rutter              | [-]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 73     | CanFieldCap_mm       | Canopy Field Capacity - Rutter                     | [mm]    |
        +--------+----------------------+----------------------------------------------------+---------+
        | 74     | DrainCoeff_mm_hr     | Drainage Coefficient - Rutter                      | [mm/hr] |
        +--------+----------------------+----------------------------------------------------+---------+
        | 75     | DrainExpPar_1_mm     | Drainage Exponent Parameter - Rutter               | [mm-1]  |
        +--------+----------------------+----------------------------------------------------+---------+
        | 76     | LandUseAlb_[]        | Albedo                                             | [-]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 77     | VegHeight_m          | Vegetation Height                                  | [m]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 78     | OptTransmCoeff_[]    | Optical Transmission Coefficient                   | [-]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 79     | StomRes_s_m          | Canopy-Average Stomatal Resistance                 | [s/m]   |
        +--------+----------------------+----------------------------------------------------+---------+
        | 80     | VegFraction_[]       | Vegetation Fraction                                | [-]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 81     | LeafAI_[]            | Canopy Leaf Area Index                             | [-]     |
        +--------+----------------------+----------------------------------------------------+---------+
        | 82     | RootZoneDepth_m      | Rootzone Depth                                     | [m]     |
        +--------+----------------------+----------------------------------------------------+---------+

Basin-averaged Hydrological Time Series
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  **Table 8.7** Content of *.mrf file

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

  **Table 8.8** Content for *.rft files

        .. tabularcolumns:: |c|c|c|c|

        +--------+-------------+----------------------------+--------+
        | Column | Variable    | Description                | Units  |
        +--------+-------------+----------------------------+--------+
        | 1      | Time_hr     | Time                       | [hr]   |
        +--------+-------------+----------------------------+--------+
        | 2      | Hsrf_m3_s   | Infiltration-excess Runoff | [m3/s] |
        +--------+-------------+----------------------------+--------+
        | 3      | Sbsrf_m3_s  | Saturation-excess Runoff   | [m3/s] |
        +--------+-------------+----------------------------+--------+
        | 4      | Psrf_m3_s   | Perched Return Flow        | [m3/s] |
        +--------+-------------+----------------------------+--------+
        | 5      | Satsrf_m3_s | Groundwater Exfiltration   | [m3/s] |
        +--------+-------------+----------------------------+--------+


Spatial Output
----------------

Dynamic Spatial Output Tables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  **Table 8.9** Content of *timestamp_00d files

  By default, all of the variables below are written to the dynamic spatial output files. The optional *DYNVARFILE* keyword (see :doc:`Model Input File`) points to a plain, CSV text file listing a subset of these variable names (Nwt,Mu,Mi,...), to reduce the number of columns written and the associated memory/disk cost; the *ID* column is always included regardless of selection. Names in *DYNVARFILE* must match the *Variable* column below exactly.

        .. tabularcolumns:: |c|c|c|c|

        +--------+----------------+------------------------------------------------+---------+
        | Column | Variable       | Description                                    | Units   |
        +--------+----------------+------------------------------------------------+---------+
        | 1      | ID             | Node Identification                            | [id]    |
        +--------+----------------+------------------------------------------------+---------+
        | 2      | Nwt            | Depth to groundwater table                     | [mm]    |
        +--------+----------------+------------------------------------------------+---------+
        | 3      | Mu             | Total moisture above the water table           | [mm]    |
        +--------+----------------+------------------------------------------------+---------+
        | 4      | Mi             | Moisture content in the initialization profile | [mm]    |
        +--------+----------------+------------------------------------------------+---------+
        | 5      | Nf             | Wetting front depth                            | [mm]    |
        +--------+----------------+------------------------------------------------+---------+
        | 6      | Nt             | Top front depth                                | [mm]    |
        +--------+----------------+------------------------------------------------+---------+
        | 7      | Qpout          | Unsaturated lateral flow out from cell         | [mm/hr] |
        +--------+----------------+------------------------------------------------+---------+
        | 8      | Qpin           | Unsaturated lateral flow into cell             | [mm/hr] |
        +--------+----------------+------------------------------------------------+---------+
        | 9      | Srf            | Surface Runoff                                 | [mm]    |
        +--------+----------------+------------------------------------------------+---------+
        | 10     | Rain           | Rainfall                                       | [mm/hr] |
        +--------+----------------+------------------------------------------------+---------+
        | 11     | ST             | Snow Temperature                               | [C]     |
        +--------+----------------+------------------------------------------------+---------+
        | 12     | IWE            | Ice Part of Snow Water Equivalent              | [cm]    |
        +--------+----------------+------------------------------------------------+---------+
        | 13     | LWE            | Liquid Part of Snow Water Equivalent           | [cm]    |
        +--------+----------------+------------------------------------------------+---------+
        | 14     | SnSub          | Snow Sublimation                               | [cm]    |
        +--------+----------------+------------------------------------------------+---------+
        | 15     | SnEvap         | Snow Evaporation                               | [cm]    |
        +--------+----------------+------------------------------------------------+---------+
        | 16     | SnMelt         | Snow Melt                                      | [cm]    |
        +--------+----------------+------------------------------------------------+---------+
        | 17     | SnDepth        | Snow Depth                                     | [cm]    |
        +--------+----------------+------------------------------------------------+---------+
        | 18     | Upack          | Internal Energy of Snow Pack                   | [kJ/m2] |
        +--------+----------------+------------------------------------------------+---------+
        | 19     | sLHF           | Latent Heat Flux from Snow Cover               | [kJ/m2] |
        +--------+----------------+------------------------------------------------+---------+
        | 20     | sSHF           | Sensible Heat Flux from Snow Cover             | [kJ/m2] |
        +--------+----------------+------------------------------------------------+---------+
        | 21     | sGHF           | Ground Heat Flux from Snow Cover               | [kJ/m2] |
        +--------+----------------+------------------------------------------------+---------+
        | 22     | sPHF           | Precipitation Heat Flux from Snow Cover        | [kJ/m2] |
        +--------+----------------+------------------------------------------------+---------+
        | 23     | sRLo           | Outgoing Longwave Radiation from Snow Cover    | [kJ/m2] |
        +--------+----------------+------------------------------------------------+---------+
        | 24     | sRLi           | Incoming Longwave Radiation from Snow Cover    | [kJ/m2] |
        +--------+----------------+------------------------------------------------+---------+
        | 25     | sRSi           | Incoming Shortwave Radiation from Snow Cover   | [kJ/m2] |
        +--------+----------------+------------------------------------------------+---------+
        | 26     | Uerr           | Error in Energy Balance                        | [J/m2]  |
        +--------+----------------+------------------------------------------------+---------+
        | 27     | IntSWE         | Intercepted Snow Water Equivalent              | [cm]    |
        +--------+----------------+------------------------------------------------+---------+
        | 28     | IntSub         | Sublimated Snow from Canopy                    | [cm]    |
        +--------+----------------+------------------------------------------------+---------+
        | 29     | IntUnl         | Unloaded Snow from Canopy                      | [cm]    |
        +--------+----------------+------------------------------------------------+---------+
        | 30     | SoilMoist      | Soil Moisture, top 10 cm                       | [-]     |
        +--------+----------------+------------------------------------------------+---------+
        | 31     | RootMoist      | Root Zone Moisture, top 1 m                    | [-]     |
        +--------+----------------+------------------------------------------------+---------+
        | 32     | CanStorage     | Canopy Storage                                 | [mm]    |
        +--------+----------------+------------------------------------------------+---------+
        | 33     | TransDryCan    | Canopy Transpiration                           | [mm/hr] |
        +--------+----------------+------------------------------------------------+---------+
        | 34     | EvpSoil        | Evaporation from Bare Soil                     | [mm/hr] |
        +--------+----------------+------------------------------------------------+---------+
        | 35     | ET             | Total Evapotranspiration                       | [mm/hr] |
        +--------+----------------+------------------------------------------------+---------+
        | 36     | GFlux          | Ground Heat Flux                               | [W/m2]  |
        +--------+----------------+------------------------------------------------+---------+
        | 37     | HFlux          | Sensible Heat Flux                             | [W/m2]  |
        +--------+----------------+------------------------------------------------+---------+
        | 38     | LFlux          | Latent Heat Flux                               | [W/m2]  |
        +--------+----------------+------------------------------------------------+---------+
        | 39     | Qstrm          | Discharge                                      | [m3/s]  |
        +--------+----------------+------------------------------------------------+---------+
        | 40     | Hlev           | Channel Stage                                  | [m]     |
        +--------+----------------+------------------------------------------------+---------+
        | 41     | FlwVlc         | Channel Flow Velocity                          | [m/s]   |
        +--------+----------------+------------------------------------------------+---------+
        | 42     | ThroughFall    | Free Throughfall Coefficient - Rutter          | [-]     |
        +--------+----------------+------------------------------------------------+---------+
        | 43     | CanFieldCap    | Canopy Field Capacity - Rutter                 | [mm]    |
        +--------+----------------+------------------------------------------------+---------+
        | 44     | DrainCoeff     | Drainage Coefficient - Rutter                  | [mm/hr] |
        +--------+----------------+------------------------------------------------+---------+
        | 45     | DrainExpPar    | Drainage Exponent Parameter - Rutter           | [mm-1]  |
        +--------+----------------+------------------------------------------------+---------+
        | 46     | LandUseAlb     | Albedo                                         | [-]     |
        +--------+----------------+------------------------------------------------+---------+
        | 47     | VegHeight      | Vegetation Height                              | [m]     |
        +--------+----------------+------------------------------------------------+---------+
        | 48     | OptTransmCoeff | Optical Transmission Coefficient               | [-]     |
        +--------+----------------+------------------------------------------------+---------+
        | 49     | StomRes        | Canopy-Average Stomatal Resistance             | [s/m]   |
        +--------+----------------+------------------------------------------------+---------+
        | 50     | VegFraction    | Vegetation Fraction                            | [-]     |
        +--------+----------------+------------------------------------------------+---------+
        | 51     | LeafAI         | Canopy Leaf Area Index                         | [-]     |
        +--------+----------------+------------------------------------------------+---------+


Time-integrated Spatial Output Table
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  **Table 8.10** Content of *timestamp_00i file

        .. tabularcolumns:: |c|c|c|c|

        +--------+------------------+---------------------------------------------------------+-----------------+
        | Column | Variable         | Description                                             | Units           |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 1      | ID               | Node Identification                                     | [id]            |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 2      | BndCd            | Boundary Flag                                           | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 3      | Proc             | Owning Processor Rank (0 in serial)                     | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 4      | Z                | Elevation                                               | [m]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 5      | VAr              | Voronoi Area                                            | [m2]            |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 6      | CAr              | Contributing Area                                       | [km2]           |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 7      | Curv             | Curvature                                               | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 8      | EdgL             | Flow Edge Length                                        | [m]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 9      | Slp              | Tangent of Flow Edge Slope                              | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 10     | FWidth           | Width of Voronoi Flow Window                            | [m]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 11     | Aspect           | Site Aspect as Angle from North                         | [radian]        |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 12     | SV               | Sky View Factor                                         | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 13     | LV               | Land View Factor                                        | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 14     | AvSM             | Average Soil Moisture, top 10 cm                        | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 15     | AvRtM            | Average Root Zone Moisture, top 1 m                     | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 16     | HOccr            | Infiltration-excess Runoff Occurrences                  | [# of TIMESTEP] |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 17     | HRt              | Infiltration-excess Runoff Average Rate                 | [mm/hr]         |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 18     | SbOccr           | Saturation-excess Runoff Occurrences                    | [# of TIMESTEP] |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 19     | SbRt             | Saturation-excess Runoff Average Rate                   | [mm/hr]         |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 20     | POccr            | Perched Return Runoff Occurrences                       | [# of TIMESTEP] |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 21     | PRt              | Perched Return Runoff Average Rate                      | [mm/hr]         |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 22     | SatOccr          | Groundwater Exfiltration Runoff Occurrences             | [# of GWSTEP]   |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 23     | SatRt            | Groundwater Exfiltration Runoff Average Rate            | [mm/hr]         |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 24     | SoiSatOccr       | Soil Saturation Occurrences                             | [# of TIMESTEP] |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 25     | RchDsch          | Recharge-Discharge Variable                             | [m]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 26     | AvET             | Average Evapotranspiration                              | [mm/hr]         |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 27     | EvpFrct          | Evaporative Fraction                                    | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 28     | cET              | Cumulative Evapotranspiration                           | [mm]            |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 29     | cEsoil           | Cumulative Soil Evaporation                             | [mm]            |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 30     | cLHF             | Cumulative Latent Heat Flux from Snow Cover             | [kJ/m2]         |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 31     | cMelt            | Cumulative Melt                                         | [cm]            |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 32     | cSHF             | Cumulative Sensible Heat Flux from Snow Cover           | [kJ/m2]         |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 33     | cPHF             | Cumulative Precipitation Heat Flux from Snow Cover      | [kJ/m2]         |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 34     | cRLIn            | Cumulative Incoming Longwave Radiation from Snow Cover  | [kJ/m2]         |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 35     | cRLo             | Cumulative Outgoing Longwave Radiation from Snow Cover  | [kJ/m2]         |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 36     | cRSIn            | Cumulative Incoming Shortwave Radiation from Snow Cover | [kJ/m2]         |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 37     | cGHF             | Cumulative Ground Heat Flux from Snow Cover             | [kJ/m2]         |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 38     | cUErr            | Cumulative Energy Balance Error                         | [kJ/m2]         |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 39     | cHrsSun          | Cumulative Hours of Sun Exposure                        | [hr]            |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 40     | cHrsSnow         | Cumulative Hours Snow Covered                           | [hr]            |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 41     | persTime         | Longest Time of Continuous Snow Cover                   | [hr]            |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 42     | peakWE           | Maximum Season SWE                                      | [cm]            |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 43     | initTime         | Simulation Hour of Initial SWE                          | [hr]            |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 44     | peakTime         | Simulation Hour of Maximum SWE                          | [hr]            |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 45     | cIntSub          | Cumulative Sublimated Snow from Canopy                  | [cm]            |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 46     | cSnSub           | Cumulative Sublimation from Snow Pack                   | [cm]            |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 47     | cSnEvap          | Cumulative Evaporation from Snow Pack                   | [cm]            |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 48     | cIntUnl          | Cumulative Unloaded Snow from Canopy                    | [cm]            |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 49     | AvTF             | Average Free Throughfall Coefficient - Rutter           | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 50     | AvCanFieldCap    | Average Canopy Field Capacity - Rutter                  | [mm]            |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 51     | AvDrainCoeff     | Average Drainage Coefficient - Rutter                   | [mm/hr]         |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 52     | AvDrainExpPar    | Average Drainage Exponent Parameter - Rutter            | [mm-1]          |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 53     | AvLUAlb          | Average Albedo                                          | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 54     | AvVegHeight      | Average Vegetation Height                               | [m]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 55     | AvOTCoeff        | Average Optical Transmission Coefficient                | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 56     | AvStomRes        | Average Canopy-Average Stomatal Resistance              | [s/m]           |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 57     | AvVegFract       | Average Vegetation Fraction                             | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 58     | AvLeafAI         | Average Canopy Leaf Area Index                          | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 59     | AvEvapThresh     | Average Soil Evaporation Threshold                      | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 60     | AvTransThresh    | Average Vegetation Transpiration Threshold              | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 61     | Bedrock_Depth_mm | Depth to Bedrock                                        | [mm]            |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 62     | Ks               | Saturated Hydraulic Conductivity                        | [mm/hr]         |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 63     | ThetaS           | Saturated Soil Moisture                                 | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 64     | ThetaR           | Residual Soil Moisture                                  | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 65     | PoreSize         | Pore Distribution Index                                 | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 66     | AirEBubPress     | Air Entry Bubbling Pressure                             | [mm] (negative) |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 67     | DecayF           | Hydraulic Decay Parameter                               | [1/mm]          |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 68     | SatAnRatio       | Saturated Anisotropy Ratio                              | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 69     | UnsatAnRatio     | Unsaturated Anisotropy Ratio                            | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 70     | Porosity         | Porosity                                                | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 71     | VolHeatCond      | Volumetric Heat Conductivity                            | [J/msK]         |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 72     | SoilHeatCap      | Soil Heat Capacity                                      | [J/m3K]         |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 73     | SoilID           | Soil Class                                              | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 74     | LandUseID        | Landuse Class                                           | [-]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
        | 75     | AvRootZoneDepth  | Average Rootzone Depth                                  | [m]             |
        +--------+------------------+---------------------------------------------------------+-----------------+
