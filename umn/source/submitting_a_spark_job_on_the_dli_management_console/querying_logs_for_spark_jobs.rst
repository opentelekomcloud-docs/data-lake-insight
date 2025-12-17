:original_name: dli_01_0379.html

.. _dli_01_0379:

Querying Logs for Spark Jobs
============================

Scenario
--------

DLI job buckets are used to store temporary data generated during DLI job running, such as job logs and results.

This section describes how to configure a bucket for DLI jobs on the DLI console and obtain Spark job logs.

Notes
-----

-  To avoid disordered job results, do not use the OBS bucket configured for DLI jobs for any other purposes.
-  DLI jobs must be set and modified by the main account as IAM users do not have required permissions.
-  You cannot view the logs for DLI jobs before configuring a bucket.
-  You can configure lifecycle rules to periodically delete objects from buckets or change storage classes of objects.
-  Exercise caution when modifying the job bucket, as it may result in the inability to retrieve historical data.
-  Spark log splitting rules:

   -  Split by size: By default, each log file has a maximum size of 128 MB.
   -  Split by time: A new log file is automatically created every hour.

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


Querying Logs for Spark Jobs
----------------------------

#. Log in to the DLI console. In the navigation pane on the left, choose **Job Management** > **Spark Jobs**.

#. Select the Spark job whose jobs you want to query, click **More** in the **Operation** column, and select **View Log**.

   The system automatically switches to the log path of the DLI job bucket.

#. On the **Files** tab, select the log file of the desired date and time and click **Download** in the **Operation** column to download the file to your local host.

.. |image1| image:: /_static/images/en-us_image_0000001995796834.png
.. |image2| image:: /_static/images/en-us_image_0000002032236253.png
