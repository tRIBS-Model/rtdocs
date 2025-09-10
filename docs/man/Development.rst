Development
=====================

tRIBS model development is driven by research, testing, and updates in software standards. We document information about the latest release in :doc:`Release Notes`. Technical issues related to tRIBS code are logged using `GitHub Issues <https://github.com/tRIBS-Model/tRIBS/issues>`_. Additional resource about model setup and applications can be found through the `tRIBS User Google Group <https://groups.google.com/g/tribs>`_. The remaining portion of this section is dedicated to describing the formal steps involved in tRIBS development and release of new versions. If you are interested in working with the tRIBS source code please see :doc:`Contributing` and :doc:`Using GitHub`.

Support
-------
We want to emphasize that tRIBS is a research related product developed and maintained in academic environment. Consequently, there is no official support for the tRIBS model. You are welcome to use tRIBS in your own research endeavors, however, the model code comes with **no guarantees, expressed or implied, as to suitability, completeness, accuracy, and whatever other claim you would like to make**. Additionally, we cannot provide individual support in setting up and running the model. This website includes reasonably complete instructions on how to run the model, including benchmark cases with full setups for example. See :doc:`Model Design` and :doc:`Benchmarks`. Finally, as an open source code we rely on the community for helping drive model development and improvements. Please refer to :doc:`Contributing` for more information on how you can contribute to tRIBS.


Versioning
----------
tRIBS follows `Semantic Versioning <https://semver.org/spec/v2.0.0.html>`_ nomenclature for model versions. In summary, the naming convention is as follows: tRIBS MAJOR.MINOR.PATCH.

* MAJOR version number is updated when ever a backward incompatible change is introduced.
* MINOR version number is updated when new feature or functions are added, but are backwards compatible.
* PATCH version number is updated when backwards-compatible bugs are fixed.

tRIBS Release
-------------
Anytime there is a MAJOR, MINOR, or PATCH update the following steps are performed.

1) The updated/fixed branch is merged with the main tRIBS branch through a pull request. This requires review and approval from the tRIBS maintainers. For more detail see :doc:`Using GitHub`.

2) We are developing various tests, from testing code compilation, end-to-end tests, and science tests. As these are developed, any new changes will be tested following these protocols.

3) With the completion of successful tests, a new build of the docker image and updates to documentation will be implemented. Note: At a minimum the :doc:`Release Notes` must be updated.

4) The release will be finalized on GitHub with ``git tag -a v<Major.Minor.Patch> -m "example of tagging"``, where previous releases can be identified by searching these tags.


