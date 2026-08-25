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

Station (Point) Data
~~~~~~~~~~~~~~~~~~~~

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

Note the following for all MDF files: the parameter names must be placed in the header row, there must be one row per timestep following the header, missing data must be inputted with the *NO_DATA* flag *= 9999.99* (interpolated internally), and units must be retained as indicated. A column-count mismatch against the expected header causes the model to exit with an error at startup. Notice that the file does not contain a minute column; see `Sub-hourly Forcings`_ below for how sub-hourly data is supplied.

Sub-hourly Forcings
~~~~~~~~~~~~~~~~~~~

Both rainfall and meteorological forcing can be supplied at sub-hourly resolution, but only as point data from stations. Gridded input, whether rainfall or meteorological, is read at hourly resolution.

Two keywords in the Model Input File (``*.in``) set the input intervals, and they are set independently of one another:

* *RAININTRVL* sets the rainfall input interval, **in hours**. For sub-hourly rain gauge data this is a fraction: ``0.25`` for 15-minute data, ``0.5`` for 30-minute data.
* *METSTEP* sets the meteorological input interval, **in minutes**. For sub-hourly weather station data this is ``15`` for 15-minute data, ``30`` for 30-minute data.

The sub-hourly interval is not arbitrary: it must be a multiple of the unsaturated zone computational time step set by *TIMESTEP*. With the default *TIMESTEP* of 3.75 minutes, this permits intervals such as 7.5, 15 and 30 minutes, but not 10 minutes. If a finer or unconventional interval is needed, *TIMESTEP* must be set so that the desired input interval remains a multiple of it.

Because *RAININTRVL* and *METSTEP* are set independently, the rainfall and meteorological inputs do not have to share a resolution. Hourly meteorological forcing can be combined with sub-hourly precipitation, or sub-hourly meteorological forcing with hourly precipitation, by setting each keyword to match the data actually provided.

Neither the Weather Station MDF (**Table 4.4**) nor the Rain Gauge MDF (**Table 4.5**) has a minute column. Sub-hourly data is instead supplied by repeating the *Hour* value across consecutive rows, one row per interval, in chronological order. For 15-minute data, four consecutive rows carry the same *Year*, *Month*, *Day* and *Hour*, and the model assigns them to the four quarters of that hour in the order they appear:

        .. tabularcolumns::  |c|c|c|c|c|

        +--------+---------+-------+--------+--------------+
        | *Year* | *Month* | *Day* | *Hour* | *Rain_mm/hr* |
        +--------+---------+-------+--------+--------------+
        | 2024   | 6       | 1     | 0      | 0.0          |
        +--------+---------+-------+--------+--------------+
        | 2024   | 6       | 1     | 0      | 2.4          |
        +--------+---------+-------+--------+--------------+
        | 2024   | 6       | 1     | 0      | 3.6          |
        +--------+---------+-------+--------+--------------+
        | 2024   | 6       | 1     | 0      | 1.2          |
        +--------+---------+-------+--------+--------------+
        | 2024   | 6       | 1     | 1      | 0.8          |
        +--------+---------+-------+--------+--------------+

Because the rows are matched by position rather than by an explicit minute stamp, the row ordering within each hour is significant and no intervals may be skipped: an hour of 15-minute data must contain exactly four rows, even where the values are zero.

Gridded Forcings
~~~~~~~~~~~~~~~~

Rainfall and meteorological forcing can each be supplied as a time series of ASCII grids instead of station data. The two are configured separately, so one may be gridded while the other is point data.

**Gridded rainfall.** Gridded (radar) rainfall is selected with *RAINSOURCE = 1*, while rain gauge station data is *RAINSOURCE = 2*. Rather than listing the individual grids, two keywords in the Input File describe how their filenames are constructed:

* *RAINFILE* gives the base pathname of the grid series, including the basename of the files but not the timestamp or extension, for example ``data/model/met/rain/radar_``.
* *RAINEXTENSION* gives the file extension used by the grids, without the leading period, for example ``asc``.

**Gridded meteorology.** Gridded meteorological data is selected with the keyword *METDATAOPTION = 2*, while the more traditional weather station data is specified with *METDATAOPTION = 1*. Here the information is provided through a text file for reading in meteorological input (``*.gdf``) as specified through the keyword *HYDROMETGRID* in the Input File, with one row per variable. The structure of the Grid Data File or GDF is presented in **Table 4.7**.

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

**Grid filenames.** Neither the rainfall nor the meteorological configuration lists individual grid files. In both cases the model constructs each filename at run time by appending a timestamp to the base name and then the extension:

.. code-block:: text

   <base name><MMDDYYYYHH>.<extension>

The timestamp is zero-padded month, day, four-digit year and hour. A meteorological grid with a base name of ``RH`` for 03:00 on 1 June 2002 is therefore ``RH0601200203.asc``, and a rainfall series with *RAINFILE* = ``data/model/met/rain/radar_`` and *RAINEXTENSION* = ``asc`` expects ``data/model/met/rain/radar_0601200203.asc`` for that same hour. Grids must be named to this convention or the model will not find them; the *RAINSEARCH* keyword controls how many hours the model will look ahead for a missing rainfall grid before exiting.
