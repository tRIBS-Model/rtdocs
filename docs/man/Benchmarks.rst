Benchmark Datasets
=====================

A unified set of benchmark datasets is publicly available in a dedicated `GitHub repository`_ and is permanently archived on Zenodo with a version-controlled Digital Object Identifier (DOI).

These examples are provided for users to validate their tRIBS installation by running the model and comparing their outputs against a trusted set of reference values. This process is now fully automated with a Python-based verification script included in the repository.

Verification Workflow
~~~~~~~~~~~~~~~~~~~~~~~

The recommended workflow for using the benchmarks is as follows:

1.  **Clone the Repository:** Get the latest version of the benchmarks from GitHub.
2.  **Run a Simulation:** Follow the instructions in the repository's README files to run one of the benchmark cases (e.g., the serial watershed model).
3.  **Verify the Results:** Use the provided verification script to automatically compare your model's output to the official reference values. The script provides a clear ``[PASS]`` or ``[FAIL]`` status.

   .. code-block:: bash

      python verification/verify.py watershed-scale-serial

This automated check provides immediate confidence that your tRIBS installation is working correctly.

Benchmark Descriptions
~~~~~~~~~~~~~~~~~~~~~~

The repository contains two primary benchmark cases for forested regions in northern Arizona, which test a wide range of model physics for interception, soil moisture, evapotranspiration, snowpack dynamics, and streamflow.

*   **Point-Scale (Happy Jack, AZ):** This case demonstrates the application of tRIBS in serial mode for a single point, ideal for testing vertical processes.
*   **Basin-Scale (Big Spring, AZ):** This case is a full watershed simulation that can be executed in either serial or parallel mode, testing spatial and routing processes.

.. warning::
   Not all model capabilities are executed in the benchmark datasets. A user needs to exercise caution when changing model inputs or altering the selected processes.

Accessing the Benchmarks
~~~~~~~~~~~~~~~~~~~~~~~~

*   **Primary GitHub Repository:** For the latest version of the input files and verification scripts, clone the `tRIBS Benchmarks Repository`_.

*   **Permanent Archive & Citation:** To cite the benchmarks or download the specific version associated with a tRIBS release, please use the `Zenodo Archive (DOI)`_.


.. _tRIBS Benchmarks Repository: https://github.com/tRIBS-Model/tRIBS-benchmarks
.. _Zenodo Archive (DOI): https://doi.org/10.5281/zenodo.17088972
.. _GitHub repository: https://github.com/tRIBS-Model/tRIBS-benchmarks