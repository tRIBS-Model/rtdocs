Model Parameters and Forcings
==============================

A tRIBS model application consists of a series of Voronoi polygons that discretely represent a watershed. At each TIN node associated with a Voronoi polygon, a set of governing equations describing the hydrologic and energy processes are computed. To solve these equations for each location, spatially distributed data describing surface topography, soils, vegetation and hydrometeorology are required, which can be obtained from field measurements, remote sensing data interpretation or model output such as numerical weather models.

All of the text-based parameter and forcing files described below: the soil and land-use reclassification tables, the station descriptor and data files, and the grid data files share a common CSV convention: a single required header row naming each column, followed by comma-separated values, with a column-count mismatch causing the model to exit with an error at startup. Past version of the model, each of these file types had its own bespoke layout (fixed-width fields, type/parameter-count header lines, etc.).

Model Parameters
------------------

In the case of soil and vegetation cover, different data sources can be used to assign uniform or spatially-explicit parameter values for the governing equations. The soils parameters listed in **Table 3.1** are necessary for carrying out the vertical infiltration and lateral flow redistribution, as well computing the soil heat budget within the sloped, heterogeneous, anisotropic soil columns assumed at each Voronoi polygon. Note that the parameter requirements vary with the specified hydrologic processes selected during a model run. For example, the surface heat parameters are only required if the radiation balance is computed. The order in which the soil parameters are listed in the tabular input should correspond to the list below and the units should match. Actual parameter names are not important for tabular input, whereas raster-based inputs assume slightly different naming conventions. Similar units need to be used when specifying soil parameters as raster inputs. 

        **Table 3.1** tRIBS Soil Parameter Description

        .. tabularcolumns:: |c|c|c|c|

        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  **Parameter**     |  **GDF Name**      |  **Description**                              |  **Unit**          |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *Ks*              |  *KS*              |  Saturated Hydraulic Conductivity             |  [mm/hr]           |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  :math:`\theta_S`  |  *TS*              |  Saturated Soil Moisture                      |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  :math:`\theta_R`  |  *TR*              |  Residual Soil Moisture                       |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *m*               |  *PI*              |  Pore Distribution Index                      |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *PsiB*            |  *PB*              |  Air Entry Bubbling Pressure                  |  [mm] (negative)   |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *f*               |  *FD*              |  Hydraulic Decay Parameter                    |  [1/mm]            |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *As*              |  *AR*              |  Saturated Anisotropy Ratio                   |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *Au*              |  *US*              |  Unsaturated Anisotropy Ratio                 |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *n*               |  *PO*              |  Porosity                                     |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *ks*              |  *VH*              |  Volumetric Heat Conductivity                 |  [J/msK]           |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *Cs*              |  *SH*              |  Soil Heat Capacity                           |  [J/m3K]           |
        +--------------------+--------------------+-----------------------------------------------+--------------------+

The vegetation or land-use parameters in **Table 3.2** are necessary for carrying out the rainfall interception, bare soil evaporation and evapotranspiration within each Voronoi polygon, as well as determining the radiation and energy balance within the land surface. Note that the parameter requirements vary with the specified processes selected during a model run. For example, rainfall interception can be computed using a simple canopy storage method or the more complex Rutter model. The order in which the vegetation parameters are listed in the tabular input should correspond to the list below and the units should match. Actual parameter names are not important for tabular input, whereas raster-based inputs assume slightly different naming conventions. Similar units need to be used when specifying vegetation parameters as raster inputs. 

        **Table 3.2** tRIBS Vegetation or Land Use Description

        .. tabularcolumns:: |c|c|c|c|

        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  **Parameter**     |  **GDF Name**      |  **Description**                              |  **Unit**          |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *P*               |  *TF*              |  Free Throughfall Coefficient - Rutter Method |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *S*               |  *CC*              |  Canopy Field Capacity - Rutter Method        |  [mm]              |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *K*               |  *DC*              |  Drainage Coefficient - Rutter Method         |  [mm/hr]           |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *b2*              |  *DE*              |  Drainage Exponent - Rutter Method            |  [1/mm]            |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *Al*              |  *AL*              |  Albedo                                       |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *h*               |  *VH*              |  Vegetation Height                            |  [m]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *Kt*              |  *OT*              |  Optical Transmission Coefficient             |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *Rs*              |  *SR*              |  Stomata Resistance                           |  [s/m]             |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *V*               |  *VF*              |  Vegetation Fraction                          |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *LAI*             |  *LA*              |  Leaf Area Index                              |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        | :math:`\theta^*_s` |  *SE*              |   Stress Threshold for Evaporation            |  [-]               |
        |                    |                    |   [:math:`\theta_R` to :math:`\theta_S`]      |                    |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        | :math:`\theta^*_t` |  *ST*              |   Stress Threshold for Transpiration          |  [-]               |
        |                    |                    |   [:math:`\theta_R` to :math:`\theta_S`]      |                    |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *RZD*             |  *RZ*              |  Rootzone Depth                               |  [m]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+

*RZD* (rootzone depth) is used to compute transpiration stress in odler versions this was hardcoded to 1 m. It does not change any physicial characteristic of the soil, it is the depth from the land surface used to compute soil mositure that is available to vegetation. A value of *9999.99* falls back to a default of 1 m. The model enforces *RZD* to be less that the depth to bedrock at each node.

Input Formats
~~~~~~~~~~~~~~

One form of data input for soil textural and vegetation data is through the use of ASCII grids consisting of soil or land use codes or indices. The soil and land use grids are specified in the Input File by using the keywords *SOILMAPNAME* and *LANDMAPNAME*. **Table 3.3** presents an example of a soil or land use grid. As with other grid input, care should be taken to specify the grids in the same coordinate system as the topographic TIN data. 

            **Table 3.3** Example of Soil or Land Use Class ASCII grid (``*.soi`` and ``*.lan``)

            .. tabularcolumns:: |c|c|c|c|c|c|

            +----------------+---------+---------+---------+---------+---------+
            |  nrows         |    6    |                                       |
            +----------------+---------+---------+---------+---------+---------+
            |  ncols         |    6    |                                       |
            +----------------+---------+---------+---------+---------+---------+
            |  xllcorner     | 346035  |                                       |
            +----------------+---------+---------+---------+---------+---------+
            |  yllcorner     | 3979905 |                                       |
            +----------------+---------+---------+---------+---------+---------+
            |  cellsize      |   2000  |                                       |
            +----------------+---------+---------+---------+---------+---------+
            |  NODATA_value  |  -9999  |                                       |
            +----------------+---------+---------+---------+---------+---------+
            |  -9999         |  -9999  |  -9999  |    1    |    0    |  -9999  |
            +----------------+---------+---------+---------+---------+---------+
            |  -9999         |  -9999  |    1    |    0    |    0    |  -9999  |
            +----------------+---------+---------+---------+---------+---------+
            |  -9999         |  -9999  |    1    |    0    |    1    |    1    |
            +----------------+---------+---------+---------+---------+---------+
            |  -9999         |    0    |    1    |    1    |  -9999  |  -9999  |
            +----------------+---------+---------+---------+---------+---------+
            |    0           |    0    |  -9999  |  -9999  |  -9999  |  -9999  |
            +----------------+---------+---------+---------+---------+---------+

The parameter values for the soil and land use grids are read from a reclassification table inputted separately using the keywords *SOILTABLENAME* and *LANDTABLENAME*. The soil reclassification (``*.sdt``) and land use reclassification (``*.ldt``) tables are plain CSV files with a single required header row naming each column, as shown in **Table 3.4** and **Table 3.5**. The header row is followed by one line per cover type, with one comma-separated value per parameter. The order and units of these parameters are **fixed**; a column-count mismatch against the expected header causes the model to exit with an error at startup. Since parameter values outside the appropriate range may result in inaccurate calculations, the user should be careful to select realistic values from literature sources prior to model use.

            **Table 3.4** Soil Reclassification Table Structure (``*.sdt``)

            .. tabularcolumns:: |c|c|c|c|c|c|c|c|c|c|c|c|

            +------+------------+-------------+-------------+--------+-----------+----------+---------+---------+--------+------------+------------+
            | *ID* | *Ks_mm/hr* | *ThetaS_[]* | *ThetaR_[]* | *m_[]* | *PsiB_mm* | *f_1/mm* | *As_[]* | *Au_[]* | *n_[]* | *ks_J/msK* | *Cs_J/m3K* |
            +------+------------+-------------+-------------+--------+-----------+----------+---------+---------+--------+------------+------------+
            | 1    | ...        | ...         | ...         | ...    | ...       | ...      | ...     | ...     | ...    | ...        | ...        |
            +------+------------+-------------+-------------+--------+-----------+----------+---------+---------+--------+------------+------------+

            **Table 3.5** Land Use Reclassification Table Structure (``*.ldt``)

            .. raw:: html

               <div style="overflow-x: auto;">

            .. tabularcolumns:: |c|c|c|c|c|c|c|c|c|c|c|c|c|c|

            +------+--------+--------+-----------+-----------+---------+-------+---------+----------+--------+----------+---------------+---------------+---------+
            | *ID* | *P_[]* | *S_mm* | *K_mm/hr* | *b2_1/mm* | *Al_[]* | *h_m* | *Kt_[]* | *Rs_s/m* | *V_[]* | *LAI_[]* | *ThetaS\*_[]* | *ThetaT\*_[]* | *RZD_m* |
            +------+--------+--------+-----------+-----------+---------+-------+---------+----------+--------+----------+---------------+---------------+---------+
            | 1    | ...    | ...    | ...       | ...       | ...     | ...   | ...     | ...      | ...    | ...      | ...           | ...           | ...     |
            +------+--------+--------+-----------+-----------+---------+-------+---------+----------+--------+----------+---------------+---------------+---------+

            .. raw:: html

               </div>

Note that the soil parameters relate to the hydraulic and thermal properties in the upper portions of the soil profile. Most of these can be directly related to the surface soil texture. The first nine parameters are essential for running the Unsaturated Zone Model while the last two are required if the keyword *GFLUXOPTION = 1*. Note that these land use parameters relate to the interception and evaporation properties of the vegetative cover or land use type. The first parameter is required if *OPTINTERCEPT = 1*. The following parameters are required for various options of the keyword *OPTEVAPOTRANS*. :math:`\theta^*_s` and :math:`\theta^*_t` specify the soil moisture stress threshold for soil evaporation and plant transpiration in units of relative soil moisture (varying from 0 to 1). The final column, *RZD* (rootzone depth, m), falls back to a default of 1 m when set to *9999.99* (see **Table 3.2**). Land use tables created before this column existed will need it added, or the model will exit on a column-count mismatch.

Gridded soil data can be used as an alternative to the tabular soil parameter input. To activate the use of the gridded soil data the user must the keyword *OPTSOILTYPE = 1* in the Input File (``*.in``). If *OPTSOILTYPE = 0* then the use of the tabular data will be selected. The information is provided through the use of a text file for reading soil grid input (``*.gdf``) specified through the keyword *SCGRID*. The structure of the soil grid data file or GDF is shown in **Table 3.6**. 

    **Table 3.6** Soil Parameter GDF File Structure

            .. tabularcolumns::  |c|c|c|

            +------------+-----------------------+------------------+
            | *Variable* |  *BasePath*           |  *FileExtension* |
            +------------+-----------------------+------------------+
            | *KS*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *TS*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *TR*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *PI*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *PB*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *FD*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *AR*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *UA*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *PO*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *VH*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *SH*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+

An alternative input format type for dynamic land cover data is with the use of grid data. This option in the tRIBS model is used with the keyword *OPTLANDUSE = 1 or 2*, while the more table-based land cover is specified with *OPTLANDUSE = 0*. The use of dynamic land cover variables maybe convenient for inputting remotely sensed vegetation fields. Information is provided through a text file for reading in land cover grid input (``*.gdf``) as specified through the keyword *LUGRID* in the Input File. Note that when provided dynamic land cover grids not parameters are required, parameters not gridded can be provided in table. Most often when applying tRIBS users will only provide gridded land cover data for parameters they have detailed data of, from LiDAR for example. The structure of the Grid Data File or GDF is presented in **Table 3.7**.

    **Table 3.7** Land Cover GDF File Structure

            .. tabularcolumns::  |c|c|c|

            +------------+-----------------------+------------------+
            | *Variable* |  *BasePath*           |  *FileExtension* |
            +------------+-----------------------+------------------+
            | *AL*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *TF*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *VH*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *SR*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *VF*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *CC*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *DC*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *DE*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *OT*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *LA*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *SE*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *ST*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *RZ*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+

``*.gdf`` files are CSV with a single header row (*Variable*, *BasePath*, *FileExtension*), one row per grid variable. Each row specifies the parameter code, the file pathname of the grid (including the basename of the file) and the extension given to the particular grid. The *NO_DATA* flag is used to specify the grids that are not available for a particular parameter.

Model Forcings
----------------

In the case of hydrometeorological forcings, model inputs can be achieved in two different ways: (1) point input of hydrometeorological observations or (2) grid input of meteorological observations or numerical model results. The model can handle the meteorological forcing in the point or grid format and has internal routines to assign this information to Voronoi polygons or TIN nodes via Thiessen resampling or nearest neighbor approaches.

**Table 3.8** lists the hydrometeorological model forcings. The primary hydrometeorological parameter is rainfall at a specified temporal resolution, typically hourly. Sub-hourly forcing can be specified despite having no minute column, by simply providing the data in order using the same hour in the hour column. The requirement of the other meteorological parameters depends on the processes selected for the model run. Humidity is specified via relative humidity (*RH*) only; dew point temperature and vapor pressure are not accepted as input and are computed internally from *RH*. Prior to v6.0.0 surface temperature (*TS*) could be left out of the input, now this value is required but if not available should be set to 9999.99. Cloud cover (*XC*) and surface temperature (*TS*) are not commonly provided inputs as tRIBS will compute these internally but the option is available if the data exists.

        **Table 3.8** tRIBS Hydrometeorological Parameter Description

        .. tabularcolumns:: |c|c|c|

        +--------------------+-----------------------------------------------+--------------------+
        |  **Parameter**     |  **Description**                              |  **Unit**          |
        +--------------------+-----------------------------------------------+--------------------+
        |  *PA*              |  Atmospheric Pressure                         |  [mb]              |
        +--------------------+-----------------------------------------------+--------------------+
        |  *RH*              |  Relative Humidity                            |  [%]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *XC*              |  Sky Cover                                    |  [tenths] (0 to 10)|
        +--------------------+-----------------------------------------------+--------------------+
        |  *US*              |  Wind Speed                                   |  [m/s]             |
        +--------------------+-----------------------------------------------+--------------------+
        |  *TA*              |  Air Temperature                              |  [C]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *TS*              |  Surface Temperature                          |  [C]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *R*               |  Rainfall                                     |  [mm/hr]           |
        +--------------------+-----------------------------------------------+--------------------+
        |  *IS*              |  Incoming Solar Radiation                     |  [W/m2]            |
        +--------------------+-----------------------------------------------+--------------------+

Input Formats
~~~~~~~~~~~~~~

Meteorological input into tRIBS can from point data or grid data, depending on the data sources available. The two data inputs are treated differently in the model. **Table 3.9** shows the two forms of meteorological data input and storage.

            **Table 3.9** Meteorological Data Input Methods

            .. tabularcolumns:: |c|c|c|

            +--------------+--------------------------------------+-----------------------------------------------+
            |Characteristic|  Point Data                          |  Grid Data                                    |
            +==============+======================================+===============================================+
            |  *Input*     |*Station Descriptor File* (``*.sdf``) |*ASCII grids* (``*.txt``, ``*.lan``, ``*.soi``)|
            +--------------+--------------------------------------+-----------------------------------------------+
            |              |*Meteorological Data File* (``*.mdf``)|                                               |
            +--------------+--------------------------------------+-----------------------------------------------+
            | *Storage*    | *Assignment to storage objects*      | *Direct assignment to* ``tCNode``             |
            +--------------+--------------------------------------+-----------------------------------------------+
            |*Manipulation*|*Thiessen point resampling*           | *Grid resampling*                             |
            +--------------+--------------------------------------+-----------------------------------------------+
            | *Examples*   | ``tHydroMet``, ``tRainGauge``        | ``tRainfall``, ``tVariant``, ``tInvariant``   |
            +--------------+--------------------------------------+-----------------------------------------------+

The Station Descriptor Files (``*.sdf``) and Meteorological Data Files (``*.mdf``) are plain CSV files with a single required header row. Both weather station and rain gauge SDFs share the same five-column structure (**Table 3.10**).

        **Table 3.10.** Weather Station / Rain Gauge SDF Structure

        .. tabularcolumns::     |c|c|c|c|c|

        +------+---------------------------+------------+-----------+-------------+
        | *ID* | *DataFile*                | *Northing* | *Easting* | *Elevation* |
        +------+---------------------------+------------+-----------+-------------+
        | 1    | /data/met/station1.mdf    | ...        | ...       | ...         |
        +------+---------------------------+------------+-----------+-------------+

Note the following: *ID* must be a unique value for each station (starting at 1), *DataFile* is the MDF file path for that station (relative to the location of of the directory where the model was executed), and *Northing*/*Easting* must be in the same coordinate system as the input grids and watershed TIN.

           **Table 3.11** Weather Station MDF Structure

            .. tabularcolumns::  |c|c|c|c|c|c|c|c|c|c|c|

            +--------+---------+-------+--------+---------+----------+-------------+----------+--------+-----------+--------+
            | *Year* | *Month* | *Day* | *Hour* | *PA_mb* | *RH_pct* | *XC_tenths* | *US_m/s* | *TA_C* | *IS_W/m2* | *TS_C* |
            +--------+---------+-------+--------+---------+----------+-------------+----------+--------+-----------+--------+
            | ...    | ...     | ...   | ...    | ...     | ...      | ...         | ...      | ...    | ...       | ...    |
            +--------+---------+-------+--------+---------+----------+-------------+----------+--------+-----------+--------+

The Weather Station MDF has exactly 11 columns.

           **Table 3.12** Rain Gauge MDF Structure

            .. tabularcolumns::  |c|c|c|c|c|

            +--------+---------+-------+--------+--------------+
            | *Year* | *Month* | *Day* | *Hour* | *Rain_mm/hr* |
            +--------+---------+-------+--------+--------------+
            | ...    | ...     | ...   | ...    | ...          |
            +--------+---------+-------+--------+--------------+

           **Table 3.13** Direct/External ET Input MDF Structure (used when *OPTEVAPOTRANS = 2*)

            .. tabularcolumns::  |c|c|c|c|c|

            +--------+---------+-------+--------+------------+
            | *Year* | *Month* | *Day* | *Hour* | *ET_mm/hr* |
            +--------+---------+-------+--------+------------+
            | ...    | ...     | ...   | ...    | ...        |
            +--------+---------+-------+--------+------------+

Note the following for all MDF files: the parameter names must be placed in the header row, there must be one row per timestep following the header, missing data must be inputted with the *NO_DATA* flag *= 9999.99* (interpolated internally), and units must be retained as indicated. A column-count mismatch against the expected header causes the model to exit with an error at startup. Notice that the file does not contain a minute column. Nevertheless, sub-hourly data can be inputted into the model at intervals that are multiples of the *TIMESTEP*. For example, for 15-minute data, the user should specify four rows for each hour (same *Hour*) in order. A similar approach is taken for sub-hourly rain gauge data.

An alternative input format type for meteorological data is with the use of grid data. This option in the tRIBS model is used with the keyword *METDATAOPTION = 2*, while the more traditional weather station data is specified with *METDATAOPTION = 1*.  The additional information is provided through a text file for reading in meteorological input (``*.gdf``) as specified through the keyword *HYDROMETGRID* in the Input File. The structure of the Grid Data File or GDF is presented in **Table 3.14**.

           **Table 3.14** Meteorological GDF File Structure

            .. tabularcolumns:: |c|c|c|

            +------------+--------------------+-----------------+
            | *Variable* | *BasePath*         | *FileExtension* |
            +------------+--------------------+-----------------+
            | PA         | Grid File Pathname | Grid Extension  |
            +------------+--------------------+-----------------+
            | RH         | Grid File Pathname | Grid Extension  |
            +------------+--------------------+-----------------+
            | XC         | Grid File Pathname | Grid Extension  |
            +------------+--------------------+-----------------+
            | US         | Grid File Pathname | Grid Extension  |
            +------------+--------------------+-----------------+
            | TA         | Grid File Pathname | Grid Extension  |
            +------------+--------------------+-----------------+
            | IS         | NO_DATA            | NO_DATA         |
            +------------+--------------------+-----------------+
            | TS         | NO_DATA            | NO_DATA         |
            +------------+--------------------+-----------------+

As with the soil and land-use GDFs (**Tables 3.6, 3.7**), this file is CSV with a single header row (*Variable*, *BasePath*, *FileExtension*). The *NO_DATA* flag is used to specify that weather grids are not available for a particular parameter. All the keywords used to represent the parameters are fixed as well as the units.

Optional Modules
------------------

Beyond the core soil, land-use and hydrometeorological inputs above, tRIBS supports a small set of optional parameter files that extend model behavior. Each falls back to a documented default when its keyword is absent, so omitting them reproduces the model's baseline behavior.

Snow Parameters
~~~~~~~~~~~~~~~~~~

When the single-layer energy-balance snow module is active (*OPTSNOW = 1*), the optional snow parameter file (``*.spf``, keyword *SNOWFILENAME*) exposes a set of physical parameters that were previously hardcoded. The file uses the same keyword/value format as the Model Input File (``*.in``): a line of descriptive text followed by the parameter value on the next line. All parameters are optional and fall back to the defaults in **Table 3.15** if the file is omitted.

        **Table 3.15** tRIBS Snow Parameter File (``*.spf``) Structure

        .. tabularcolumns:: |c|c|c|c|

        +---------------------------+-------------------------------------------------------------------------------+----------+-------------+
        | **Keyword**               | **Description**                                                               | **Unit** | **Default** |
        +---------------------------+-------------------------------------------------------------------------------+----------+-------------+
        | *IRREDUCIBLE_SAT*         | Irreducible water saturation (fraction of pore space)                         | [V/V]    | 0.04        |
        +---------------------------+-------------------------------------------------------------------------------+----------+-------------+
        | *K_SAT_REF*               | Saturated hydraulic conductivity of the snowpack                              | [m/s]    | 0.005       |
        +---------------------------+-------------------------------------------------------------------------------+----------+-------------+
        | *MIN_SNOW_TEMP*           | Minimum snow temperature                                                      | [C]      | -30         |
        +---------------------------+-------------------------------------------------------------------------------+----------+-------------+
        | *FRESH_SNOW_DENSITY*      | Fresh snow density baseline                                                   | [kg/m3]  | 60          |
        +---------------------------+-------------------------------------------------------------------------------+----------+-------------+
        | *CANOPY_WIND_ATTENUATION* | Canopy wind attenuation coefficient                                           | [-]      | 0.5         |
        +---------------------------+-------------------------------------------------------------------------------+----------+-------------+
        | *ROUGHNESS_LENGTH*        | Aerodynamic roughness length (z0) of the snow surface                         | [m]      | 0.001       |
        +---------------------------+-------------------------------------------------------------------------------+----------+-------------+
        | *ALBEDO_FRESH*            | Fresh snow albedo                                                             | [-]      | 0.85        |
        +---------------------------+-------------------------------------------------------------------------------+----------+-------------+
        | *ALBEDO_DECAY_DRY*        | Albedo exponential decay rate, dry snow                                       | [-]      | 0.96        |
        +---------------------------+-------------------------------------------------------------------------------+----------+-------------+
        | *ALBEDO_DECAY_WET*        | Albedo exponential decay rate, wet snow                                       | [-]      | 0.82        |
        +---------------------------+-------------------------------------------------------------------------------+----------+-------------+
        | *ALBEDO_MIN*              | Minimum snow albedo floor                                                     | [-]      | 0.4         |
        +---------------------------+-------------------------------------------------------------------------------+----------+-------------+
        | *ALBEDO_RESET_THRESHOLD*  | Minimum snowfall depth required to reset surface age                          | [mm]     | 5           |
        +---------------------------+-------------------------------------------------------------------------------+----------+-------------+
        | *OPTPRECPARTITION*        | Precipitation phase-partitioning scheme (0 = wet-bulb, 1 = linear transition) | [-]      | 0           |
        +---------------------------+-------------------------------------------------------------------------------+----------+-------------+
        | *MAX_WETBULB_TEMP*        | Upper wet-bulb temperature limit for snowfall (OPTPRECPARTITION = 0)          | [C]      | 5           |
        +---------------------------+-------------------------------------------------------------------------------+----------+-------------+
        | *MIN_TEMP_RAIN*           | Minimum air temperature for liquid precipitation (OPTPRECPARTITION = 1)       | [C]      | 0           |
        +---------------------------+-------------------------------------------------------------------------------+----------+-------------+
        | *MAX_TEMP_SNOW*           | Maximum air temperature for snowfall (OPTPRECPARTITION = 1)                   | [C]      | 4           |
        +---------------------------+-------------------------------------------------------------------------------+----------+-------------+

*OPTPRECPARTITION* selects how precipitation phase is determined: with the default wet-bulb method (*0*), with the wet-bulb method (0), the snow fraction of precipitation decreases continuously with wet-bulb temperature and is set to zero above *MAX_WETBULB_TEMP*; with the linear-transition method (*1*), the model linearly transitions from all-snow at or below *MIN_TEMP_RAIN* to all-rain at or above *MAX_TEMP_SNOW* based on air temperature.

Monthly Stomatal Resistance Scaling
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The optional *RSPARAMFILE* keyword (see :doc:`Model Input File`) points to a CSV file that scales the minimum stomatal resistance (*Rs*, **Table 3.2**) by month, on top of the model's built-in diurnal scaling, following `(Becerra, 2026)`_. The file consists of a single header row (skipped by the model) followed by one row of 12 comma-separated monthly multipliers, in order from January to December.

        **Table 3.16** Monthly Stomatal Resistance Scaling File Structure

        .. tabularcolumns:: |c|c|c|c|c|c|c|c|c|c|c|c|

        +-------+-------+-------+-------+-------+-------+-------+-------+-------+-------+-------+-------+
        | *Jan* | *Feb* | *Mar* | *Apr* | *May* | *Jun* | *Jul* | *Aug* | *Sep* | *Oct* | *Nov* | *Dec* |
        +-------+-------+-------+-------+-------+-------+-------+-------+-------+-------+-------+-------+
        | 1.0   | 1.0   | 1.1   | 1.2   | 1.3   | 1.4   | 1.4   | 1.3   | 1.2   | 1.1   | 1.0   | 1.0   |
        +-------+-------+-------+-------+-------+-------+-------+-------+-------+-------+-------+-------+

If *RSPARAMFILE* is absent, all 12 monthly factors default to 1.0 (no seasonal scaling). The model exits with an error if the file is missing, unopenable, or does not contain exactly 12 values.

.. _(Becerra, 2026): https://hdl.handle.net/2286/R.2.N.204771

