Model Forcings
==============

Hydrometeorological forcings drive the governing equations solved at each Voronoi polygon, alongside the soil and vegetation parameters described in :doc:`Model_Parameters`. The text-based forcing files described below, the station descriptor and data files and the grid data files, share the same CSV convention as the parameter files: a single required header row naming each column, followed by comma-separated values, with a column-count mismatch causing the model to exit with an error at startup.

In the case of hydrometeorological forcings, model inputs can be achieved in two different ways: (1) point input of hydrometeorological observations or (2) grid input of meteorological observations or numerical model results. The model can handle the meteorological forcing in the point or grid format and has internal routines to assign this information to Voronoi polygons or TIN nodes via Thiessen resampling or nearest neighbor approaches.

**Table 4.1** lists the hydrometeorological model forcings. The primary hydrometeorological parameter is rainfall at a specified temporal resolution, typically hourly. Sub-hourly forcing can be specified despite having no minute column, by simply providing the data in order using the same hour in the hour column. The requirement of the other meteorological parameters depends on the processes selected for the model run. Humidity is specified via relative humidity (*RH*) only; dew point temperature and vapor pressure are not accepted as input and are computed internally from *RH*. Prior to v6.0.0 surface temperature (*TS*) could be left out of the input, now this value is required but if not available should be set to 9999.99. Cloud cover (*XC*) and surface temperature (*TS*) are not commonly provided inputs as tRIBS will compute these internally but the option is available if the data exists.

        **Table 4.1** tRIBS Hydrometeorological Parameter Description

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
-------------

Meteorological input into tRIBS can from point data or grid data, depending on the data sources available. The two data inputs are treated differently in the model. **Table 4.2** shows the two forms of meteorological data input and storage.

            **Table 4.2** Meteorological Data Input Methods

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

The Station Descriptor Files (``*.sdf``) and Meteorological Data Files (``*.mdf``) are plain CSV files with a single required header row. Both weather station and rain gauge SDFs share the same five-column structure (**Table 4.3**).

        **Table 4.3.** Weather Station / Rain Gauge SDF Structure

        .. tabularcolumns::     |c|c|c|c|c|

        +------+---------------------------+------------+-----------+-------------+
        | *ID* | *DataFile*                | *Northing* | *Easting* | *Elevation* |
        +------+---------------------------+------------+-----------+-------------+
        | 1    | /data/met/station1.mdf    | ...        | ...       | ...         |
        +------+---------------------------+------------+-----------+-------------+

Note the following: *ID* must be a unique value for each station (starting at 1), *DataFile* is the MDF file path for that station (relative to the location of of the directory where the model was executed), and *Northing*/*Easting* must be in the same coordinate system as the input grids and watershed TIN.

           **Table 4.4** Weather Station MDF Structure

            .. tabularcolumns::  |c|c|c|c|c|c|c|c|c|c|c|

            +--------+---------+-------+--------+---------+----------+-------------+----------+--------+-----------+--------+
            | *Year* | *Month* | *Day* | *Hour* | *PA_mb* | *RH_pct* | *XC_tenths* | *US_m/s* | *TA_C* | *IS_W/m2* | *TS_C* |
            +--------+---------+-------+--------+---------+----------+-------------+----------+--------+-----------+--------+
            | ...    | ...     | ...   | ...    | ...     | ...      | ...         | ...      | ...    | ...       | ...    |
            +--------+---------+-------+--------+---------+----------+-------------+----------+--------+-----------+--------+

The Weather Station MDF has exactly 11 columns.

           **Table 4.5** Rain Gauge MDF Structure

            .. tabularcolumns::  |c|c|c|c|c|

            +--------+---------+-------+--------+--------------+
            | *Year* | *Month* | *Day* | *Hour* | *Rain_mm/hr* |
            +--------+---------+-------+--------+--------------+
            | ...    | ...     | ...   | ...    | ...          |
            +--------+---------+-------+--------+--------------+

           **Table 4.6** Direct/External ET Input MDF Structure (used when *OPTEVAPOTRANS = 2*)

            .. tabularcolumns::  |c|c|c|c|c|

            +--------+---------+-------+--------+------------+
            | *Year* | *Month* | *Day* | *Hour* | *ET_mm/hr* |
            +--------+---------+-------+--------+------------+
            | ...    | ...     | ...   | ...    | ...        |
            +--------+---------+-------+--------+------------+

Note the following for all MDF files: the parameter names must be placed in the header row, there must be one row per timestep following the header, missing data must be inputted with the *NO_DATA* flag *= 9999.99* (interpolated internally), and units must be retained as indicated. A column-count mismatch against the expected header causes the model to exit with an error at startup. Notice that the file does not contain a minute column. Nevertheless, sub-hourly data can be inputted into the model at intervals that are multiples of the *TIMESTEP*. For example, for 15-minute data, the user should specify four rows for each hour (same *Hour*) in order. A similar approach is taken for sub-hourly rain gauge data.

An alternative input format type for meteorological data is with the use of grid data. This option in the tRIBS model is used with the keyword *METDATAOPTION = 2*, while the more traditional weather station data is specified with *METDATAOPTION = 1*.  The additional information is provided through a text file for reading in meteorological input (``*.gdf``) as specified through the keyword *HYDROMETGRID* in the Input File. The structure of the Grid Data File or GDF is presented in **Table 4.7**.

           **Table 4.7** Meteorological GDF File Structure

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

As with the soil and land-use GDFs (**Tables 3.6, 3.7** in :doc:`Model_Parameters`), this file is CSV with a single header row (*Variable*, *BasePath*, *FileExtension*). The *NO_DATA* flag is used to specify that weather grids are not available for a particular parameter. All the keywords used to represent the parameters are fixed as well as the units.
