Executables
===========
tRIBS executables for both macOS (compatible with Intel or Silicon chips) and Ubuntu are distributed with each tagged release on GitHub. Download the assets for the current version from the latest release page:

    https://github.com/tRIBS-Model/tRIBS/releases/latest

The executables are provided as self-extracting compressed archive files. These files are formatted as shell scripts and can be unpacked by running the file from the command line. You will be prompted with the license and the option for installing in the default ``TIN-based Real-time Integrated Basin Simulator-<version>`` directory or a ``bin`` directory. Each file contains the serial version of the model (tRIBS).

These executables are only compatible with the specific operating systems listed below. If none of them is compatible with your operating system, we recommend that you either use the tRIBS :doc:`Docker` image or build tRIBS from the source code as described in :doc:`Model_Execution`.

.. note::

   We no longer distribute a packaged parallel executable (tRIBSpar). A parallel build is linked against the MPI installation it was compiled with, so it will only run in an environment matching ours exactly. To run tRIBS in parallel, either use the tRIBS :doc:`Docker` image, which contains a parallel build, or compile tRIBS from the source code against the MPI installation on your own system, as described in :doc:`Model_Execution`.

macOS
-----------

On macOS, files downloaded from the internet are tagged with a quarantine attribute, and Gatekeeper will block the self-extracting archive from running. Remove the attribute before running the installer, for example:

.. code-block:: bash

   xattr -d com.apple.quarantine tRIBS-6.0.0-macOS-Silicon.sh

Substitute the name of the file you downloaded, then run the script as usual.

Silicon
~~~~~~~
Built on M1/Sonoma macOS.

Intel
~~~~~
Built on Intel/Ventura macOS.

Linux
-------------

Ubuntu
~~~~~~
Built on Ubuntu 22.04.
