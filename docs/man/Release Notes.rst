Release Notes
=====================

This page provide a record of changes recorded by each version of tRIBS, starting with Version 5.2.0.

Known Issues
------------
For a list of known issues and their status, visit the tRIBS GitHub `Issues page <https://github.com/tribshms/tRIBS/issues>`_.

------------------------------------------------------------------------------------------

Version History
---------------

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
