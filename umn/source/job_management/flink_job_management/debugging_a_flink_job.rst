:original_name: dli_01_0458.html

.. _dli_01_0458:

Debugging a Flink Job
=====================

The job debugging function helps you check the logic correctness of your compiled SQL statements before running a job.

.. note::

   -  Currently, only Flink SQL jobs support this function.
   -  The job debugging function is used only to verify the SQL logic and does not involve data write operations.

Procedures
----------

#. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.

#. In the **Operation** column of the created Flink SQL job, click **Edit**. The page for editing the Flink SQL job is displayed.

   For a job that is being created, you can debug the job on the editing page.

#. Click **Debug** above the SQL editing box to parse the edited SQL statements. The **Debugging Parameters** tab is displayed on the right of the page.

   -  **Dump Bucket**: Select an OBS bucket to save debugging logs. If you select an unauthorized OBS bucket, click **Authorize**.
   -  **Data Input Mode**: You can select CSV data stored in the OBS bucket or manually enter the data.

      -  **OBS (CSV)**

         If you select this value, prepare OBS data first before using DLI. For details, see :ref:`Preparing Flink Job Data <dli_01_0454>`. OBS data is stored in CSV format, where multiple records are separated by line breaks and different fields in a single record are separated by commas (,). In addition, you need to select a specific object in OBS as the input source data.

      -  **Manual typing**

         If you select this value, compile SQL statements as data sources. In this mode, you need to enter the value of each field in a single record.

#. Click **Start Debugging**. Once debugging is complete, the **Debugging Result** page appears.

   -  If the debugging result meets the expectation, the job is running properly.
   -  If the debugging result does not meet the expectation, business logic errors may have occurred. In this case, modify SQL statements and conduct debugging again.
