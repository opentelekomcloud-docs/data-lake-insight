:original_name: dli_03_0119.html

.. _dli_03_0119:

Why Does a Flink Jar Package Conflict Result in Job Submission Failure?
=======================================================================

Symptom
-------

The dependency of your Flink job conflicts with a built-in dependency of the DLI Flink platform. As a result, the job submission fails.

Solution
--------

Check whether there are conflicting JAR files.

DLI Flink provides a series of pre-installed dependency packages in the DLI service to support various data processing and analysis tasks.

If the JAR file you upload contains a package that already exists in the DLI Flink runtime platform, it will prompt a Flink Jar package conflict, resulting in job submission failure.

Delete the repetitive package by referring to the dependency package information provided in the *Data Lake Insight User Guide* and then upload the package.
