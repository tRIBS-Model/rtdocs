Release Notes
=====================

This page provide a record of changes recorded by each version of tRIBS, starting with Version 5.2.0.

Known Issues
------------

- Mid-storm restarts diverge slightly (routing queues in transit are not preserved); the restart module is intended for use at spinup or end-of-dry-period.
- ``OPTPERCOLATION = 3`` (Green-Ampt) currently exits with an error and is unavailable.
- Flux magnitudes will differ from v5.x results as a result of the water-balance corrections in the 6.0.0 release. This is expected, not a regression.

For a list of known issues and their status, visit the tRIBS GitHub `Issues page <https://github.com/tRIBS-Model/tRIBS/issues>`_.

------------------------------------------------------------------------------------------

Version History
---------------

tRIBS 6.0.0 (August 2026)
~~~~~~~~~~~~~~~~~~~~~~~~~

The tRIBS Distributed Hydrologic Modeling System, Version 6.0.0, is a major release focused on simplifying the user experience and streamlining the codebase. **It is not backwards compatible with previous versions:** the main input file, the text-based input and output formats, and several parameter conventions have all changed. Example input files are available in the `Model Benchmark Repository <https://github.com/tRIBS-Model/tRIBS-benchmarks>`_. Users working through the `pytRIBS <https://github.com/tRIBS-Model/pytRIBS>`_ Python package should upgrade to pytRIBS 1.0.0, which supports the v6.0.0 input and output formats; earlier versions of pytRIBS are not compatible with this release.

Nearly all text-based input and output is now CSV with a single header row, replacing the previous fixed-width and count-header formats. Station and gridded forcing files no longer carry centroid latitude, longitude, or UTC offset; these moved to the main input file as ``UTCOFFSET``, ``CENTROIDLAT``, and ``CENTROIDLONG``. Point and gridded rainfall both standardize on mm/hr, and relative humidity is the only accepted humidity forcing. Land use parameters are now validated at startup, so models carrying out-of-range values stop with a descriptive message rather than running on.

The snow module was refactored with dynamic snow density via compaction, liquid water routing by holding capacity and conductivity, and a selectable precipitation phase-partitioning scheme, all configured through a new optional snow parameter file (``.spf``). Canopy interception is handled exclusively by the Rutter water-balance method, with the legacy Gray (1970) formulation removed. Root zone depth became a land use parameter, supplied through the land use table or a raster, replacing the model-wide ``ROOTZONEDEPTH`` keyword, and a new static gridded land use option (``OPTLANDUSE = 2``) supplements the existing dynamic and tabular modes. Evapotranspiration, unsaturated zone, and snowpack calculations received several mass-balance corrections, and channel transmission losses were reworked, with conductivity now specified in mm/hr. Initial groundwater depth and depth to bedrock are now interpreted as vertical depths rather than slope-normal, which will change results for existing model setups.

Parallel simulations are now self-contained: METIS is vendored into the source tree and compiled into the parallel binary, and the stream-reach graph is built and partitioned in-process at startup for however many MPI processes the run was launched with, retiring the standalone MeshBuilder tool and its external ``gpmetis`` and Perl-script workflow. A new partition-only mode (``PARALLELMODE = 2``) writes the partition file and reports load balance and edge cuts without running a simulation, and parallel outputs are merged on the head node rather than written per-processor. The restart module was rewritten to save a minimal, portable state that can hot start any simulation on the same mesh, with no dependence on simulation time or the original processor count. Optional GDAL support (``-DWITH_GDAL=ON``) allows tRIBS to read GeoTIFF, NetCDF, and other GDAL-supported raster formats. The legacy rainfall forecasting mode, stochastic storm generator, and RIBS-output compatibility mode were removed, an optional monthly stomatal resistance scaling file (``RSPARAMFILE``) was added, and internal performance optimizations reduced typical wall-clock time by 15 to 20%.

tRIBS 5.3.0 (August 2025)
~~~~~~~~~~~~~~~~~~~~~~~~~

The tRIBS Distributed Hydrologic Modeling System, Version 5.3.0, represents a feature and maintenance release that introduces new user-configurable parameters and enhances the physical realism of core model components. Key advancements include optional inputs for soil moisture stress and surface layer depths, a more physically-based partitioning of evapotranspiration components, and the incorporation of an atmospheric stability correction in the snowpack energy balance. This version also addresses critical bugs related to dynamic land use handling and mesh element calculations. Substantial refactoring of the solar radiation and snow modules has been performed to improve code clarity and maintainability. These combined updates significantly expand the model's flexibility and scientific capabilities while increasing its overall robustness.

tRIBS 5.2.1 (June 2024)
~~~~~~~~~~~~~~~~~~~~~~~

The tRIBS Distributed Hydrologic Modeling System, Version 5.2.1, provides a focused patch addressing key bugs and improving model stability and reproducibility. This maintenance release includes updates to meteorological variable initialization, improvements in numerical output precision, and enhancements to build configuration flexibility. Additionally, several legacy elements and potentially misleading behaviors have been deprecated or temporarily disabled pending review. These targeted fixes aim to enhance the model's robustness and facilitate future development efforts.

tRIBS 5.2.0 (March 2024)
~~~~~~~~~~~~~~~~~~~~~~~~

The tRIBS Distributed Hydrologic Modeling System, Version 5.2.0, represents a culmination of efforts and significant improvements from the initial release [#]_ and later versions [#]_, [#]_. This latest version includes new physical processes related to level-pool reservoir routing [#]_ and channel transmission losses [#]_. In addition, the latest updates (Version 5.0 and onward) entail a CMake build system ,  major code improvements, including a refactored snow module, fixed memory leaks in parallel mode, and updates to C++ 17 standards.

.. [#] Ivanov, V.Y., Vivoni, E.R., Bras, R.L., and Entekhabi, D. 2004. Catchment Hydrologic Response with a Fully-distributed Triangulated Irregular Network Model. *Water Resources Research*. 40(11): W11102. https://doi.org/10.1029/2004WR003218

.. [#] Vivoni, E.R., Mascaro, G., Mniszewski, S., Fasel, P., Springer, E.P., Ivanov, V.Y., and Bras, R.L. 2011. Real-world Hydrologic Assessment of a Fully-Distributed Hydrological Model in a Parallel Computing Environment. *Journal of Hydrology*. 409: 483-496. https://doi.org/10.1016/j.jhydrol.2011.08.053

.. [#] Rinehart, A.J., Vivoni, E.R., and Brooks, P.D. 2008. Effects of Vegetation, Albedo and Solar Radiation Sheltering on the Distribution of Snow in the Valles Caldera, New Mexico. *Ecohydrology*. 1(3): 253-270. https://doi.org/10.1002/eco.26

.. [#] Cazares-Rodriguez, J.E., Vivoni, E.R., and Mascaro, G. 2017. Comparison of Two Watershed Models for Addressing Stakeholder Flood Mitigation Strategies: Case Study of Hurricane Alex in Monterrey, México. *Journal of Hydrologic Engineering*. 22(9): 05017018, 1-16. https://doi.org/10.1061/(ASCE)HE.1943-5584.0001560

.. [#] Schreiner-McGraw, A.P., and Vivoni, E.R. 2018. On the Sensitivity of Hillslope Runoff and Channel Transmission Losses in Arid Piedmont Slopes. *Water Resources Research*. 54(7): 4498-4518. https://doi.org/10.1029/2018WR022842
