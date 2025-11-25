:original_name: dli_01_0536.html

.. _dli_01_0536:

Configuring a DLI Job Bucket
============================

Before using DLI, you need to configure a DLI job bucket. The bucket is used to store temporary data generated during DLI job running, such as job logs and results.

Configure a DLI job bucket on the **Global Configuration** > **Project** page of the DLI management console.

.. note::

   If you have enabled the function to save job results to a DLI job bucket for your SQL queue, make sure to configure the DLI job bucket before submitting SQL jobs. Failure to do so may result in SQL jobs not being submitted successfully. For details, refer to :ref:`How Do I Check if Job Result Saving to a DLI Job Bucket Is Enabled for a SQL Queue? <dli_01_0563>`

Preparations
------------

Before the configuration, create an **OBS bucket** or **parallel file system (PFS)**.

In big data scenarios, you are advised to create a PFS. PFS is a high-performance file system provided by OBS, with access latency in milliseconds. PFS can achieve a bandwidth performance of up to TB/s and millions of IOPS, which makes it ideal for processing high-performance computing (HPC) workloads.

Notes
-----

-  Do not use the OBS bucket for other purposes.

-  The OBS bucket must be set and modified by the main account. Member users do not have the permission.

-  If the bucket is not configured, you will not be able to view job results and logs.

-  You can create lifecycle rules to automatically delete objects or change storage classes for objects that meet specified conditions.

-  Inappropriate modifications of the job bucket may lead to loss of historical data.

-  If your SQL queue has been configured to save job results to a DLI job bucket, the system will write the results directly to the specified OBS bucket when executing a SQL job. Ensure the following preparations are completed before submitting the job:

   -  Configure DLI job bucket information before submitting a SQL job. Otherwise, the SQL job may fail to be submitted.

   -  Ensure that the user executing the SQL job has read and write permissions for the DLI job bucket. Otherwise, the job results cannot be properly saved or retrieved.

      When executing a job, the system accesses the job bucket using the identity credentials of the user who submitted the job. If permissions are insufficient, it results in failure to save or retrieve the job results.

   For details, refer to :ref:`How Do I Check if Job Result Saving to a DLI Job Bucket Is Enabled for a SQL Queue? <dli_01_0563>`

Procedure
---------

#. In the navigation pane of the DLI console, choose **Global Configuration** > **Project**.

#. On the **Project** page, click |image1| next to **Job Bucket** to configure bucket information.

#. Click |image2| to view available buckets.

#. Select the bucket for storing the temporary data of the DLI job and click **OK**.

   Temporary data generated during DLI job running will be stored in the OBS bucket.

.. |image1| image:: /_static/images/en-us_image_0000001452097001.png
.. |image2| image:: /_static/images/en-us_image_0000001452099393.png
