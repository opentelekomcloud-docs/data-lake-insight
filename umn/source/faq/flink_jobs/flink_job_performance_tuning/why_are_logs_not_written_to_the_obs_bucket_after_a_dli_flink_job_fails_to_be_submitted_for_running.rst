:original_name: dli_03_0064.html

.. _dli_03_0064:

Why Are Logs Not Written to the OBS Bucket After a DLI Flink Job Fails to Be Submitted for Running?
===================================================================================================

Mode for storing generated job logs when a DLI Flink job fails to be submitted or executed. The options are as follows:

-  If the submission fails, a submission log is generated only in the **submit-client** directory.

-  You can view the logs generated within 1 minute when the job fails to be executed on the management console.

   Choose **Job Management** > **Flink Jobs**, click the target job name to go to the job details page, and click **Run Log** to view real-time logs.

-  If the running fails and exceeds 1 minute (the log dump period is 1 minute), run logs are generated in the **application\_**\ *xx* directory.

Flink dependencies have been built in the DLI server and security hardening has been performed based on the open-source community version. To avoid dependency package compatibility issues or log output and dump issues, be careful to exclude the following files when packaging:

-  Built-in dependencies (or set the package dependency scope to "provided")
-  Log configuration files (example, log4j.properties/logback.xml)
-  JAR file for log output implementation (example, log4j).

On this basis, the **taskmanager.log** file rolls as the log file size and time change.
