:original_name: dli_01_0651.html

.. _dli_01_0651:

Querying Logs for Flink Jobs
============================

Scenario
--------

DLI job buckets are used to store temporary data generated during DLI job running, such as job logs and results.

This section describes how to configure a bucket for DLI jobs on the DLI console and obtain Flink job logs.

Notes
-----

-  To avoid disordered job results, do not use the OBS bucket configured for DLI jobs for any other purposes.
-  DLI jobs must be set and modified by the main account as IAM users do not have required permissions.
-  You cannot view the logs for DLI jobs before configuring a bucket.
-  You can configure lifecycle rules to periodically delete objects from buckets or change storage classes of objects.
-  Exercise caution when modifying the job bucket, as it may result in the inability to retrieve historical data.

Prerequisites
-------------

Before the configuration, create an OBS bucket or parallel file system (PFS). In big data scenarios, you are advised to create a PFS. PFS is a high-performance file system provided by OBS, with access latency in milliseconds. PFS can achieve a bandwidth performance of up to TB/s and millions of IOPS, which makes it ideal for processing high-performance computing (HPC) workloads.

Configuring a Bucket for DLI Jobs
---------------------------------

#. In the navigation pane of the DLI console, choose **Global Configuration** > **Project**.

#. On the **Project** page, click |image1| next to **Job Bucket** to configure bucket information.

#. Click |image2| to view available buckets.

#. In the displayed **OBS** dialog box, click the name of a bucket or search for and click a bucket name and then click **OK**. In the **Set Job Bucket** dialog box, click **OK**.

   Temporary data generated during DLI job running will be stored in the OBS bucket.

Viewing Commit Logs
-------------------

You can check commit logs to locate commit faults.

#. In the navigation pane of the DLI console, choose **Job Management** > **Flink Jobs**.
#. Click the name of the Flink job whose commit logs you want to check.
#. Click the **Commit Logs** tab and check the job commit process.

Viewing Run Logs
----------------

You can check run logs to locate job running faults.

#. In the navigation pane of the DLI console, choose **Job Management** > **Flink Jobs**.

#. Click the name of the Flink job whose commit logs you want to check.

#. Click the **Run Log** tab and check the JobManager and TaskManager information of the running job.

   JobManager and TaskManager information is updated every minute. By default, the run logs generated in the last minute are displayed.

   If you have configured an OBS bucket to store job logs, you can access it to download and check historical logs.

   If the job is not running, you cannot check Task Manager information.

.. |image1| image:: /_static/images/en-us_image_0000002032233425.png
.. |image2| image:: /_static/images/en-us_image_0000002032113853.png
