:original_name: dli_03_0119.html

.. _dli_03_0119:

Why Does the Submission Fail Due to Flink JAR File Conflict?
============================================================

Symptom
-------

The dependency of your Flink job conflicts with a built-in dependency of the DLI Flink platform. As a result, the job submission fails.

Solution
--------

Delete your JAR file that is the same as an existing one of the DLI Flink platform.
