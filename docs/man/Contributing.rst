Contributing
=====================

Open Source Frame Work
----------------------
tRIBS is available as an open source software through `GitHub <https://github.com/tRIBS-Model/tRIBS>`_ and `Docker Hub <https://hub.docker.com/repository/docker/tribs/tribs/general>`_. More information can be found at :doc:`Using GitHub` and :doc:`Docker`. One of the benefits of open source software is that the underlying code is available for anyone to inspect, modify, debug, and fix. We expect that both maintainers and experienced user at some point or another will have valuable modifications or fixes for tRIBS source code. Below we highlight some resources and provide instructions for contributing to the code base.

Resources
---------
Integrated Development Environment (IDE)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
If you are going to be interacting with tRIBS source code we recommend that you use an IDE. There are some freely available IDE's such as `Visual Studio Code <https://code.visualstudio.com/download>`_. Alternatively, `CLion <https://www.jetbrains.com/clion/>`_ provides excellent resources for working with C++ for a yearly subscription fee or is free with an `educational license <https://www.jetbrains.com/community/education/#students>`_. With an IDE you can actively track variables in real time even for compiled languages, or alternatively you can use `GDB <https://sourceware.org/gdb/>`_ to debug and identify variable behavior inside tRIBS while it's executing.

Issues
~~~~~~
If you encounter an error, we first ask that you check to ensure that it is not user related. The majority of issues arise from user related errors, a result of improperly setting up the model inputs, including not specifying the right paths to important datasets or forgetting to turn on or off certain option in the input file. Though tRIBS has been developed to catch a number of user errors and provide informative error messages, we ask that if you encounter a potential error that you double check to ensure that is not user related, this can be achieved using an IDE or GDB as outlined above. If it is user related, but tRIBS does not provide an informative error message, we encourage you to still log the issue as outlined below.

If you do encounter an error that is clearly related to tRIBS syntax or semantics, we encourage you to take the following steps.

1) Visit both the `tRIBS GitHub Issue page <https://github.com/tRIBS-Model/tRIBS/issues>`_ and the `tRIBS Google Group <https://groups.google.com/g/tribs>`_ and search the sites to see if your error has been encountered before.

2) If the issue has not been documented, then feel free to create a new issue on the `tRIBS GitHub Issue page <https://github.com/tRIBS-Model/tRIBS/issues>`_ . We ask that if your issue is related to tRIBS behavior that you provide as much information as possible about the error, including details in your model setup.

3) Once the issue is flagged, you can either wait for another user or one of the maintainers to fix the problem. Or if you are able to fix the problem yourself, you can provide a pull request outlined at :doc:`Using GitHub`.

Modifications and Improvements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
If you have comments or ideas for improving tRIBS, or would like to introduce new functionality to the source code, we ask that you log these ideas under the tRIBS GitHub Milestone page: `Exploratory Ideas <https://github.com/tRIBS-Model/tRIBS/milestones>`_. Also consider starting a conversation around your ideas or comments on the `tRIBS Google Group site <https://groups.google.com/g/tribs>`_. Lastly, we would like to note that tRIBS is a large program and introducing new features or improvements can be challenging and requires significant tests as well as clear demonstration that the improvements are needed.

For minor fixes that are on the level of patches, see :doc:`Development`, we maintain a separate tRIBS GitHub Milestone page: `To Do <https://github.com/tRIBS-Model/tRIBS/milestone/2>`_ this is mostly intended as a place to log needed fixes that are not critical but are on the docket for the next PATCH update.
