:original_name: dli_01_0621.html

.. _dli_01_0621:

Exporting SQL Job Results
=========================

Store the data results of analyzed SQL jobs in a specified location in the desired format.

By default, DLI stores SQL job results in its job bucket. You can also download job results to a local host or export job results to a specified OBS bucket.

Exporting Job Results to the DLI Job Bucket
-------------------------------------------

DLI specifies a default OBS bucket for storing job results. You can configure the bucket information on the **Global Configuration** > **Project** page of the DLI management console. Once a job is complete, the system automatically stores its results to this bucket.

The following conditions must be met if you want to read job results from the DLI job bucket:

-  You have configured the job bucket on the **Global Configuration** > **Project** page of the DLI management console by referring to :ref:`Configuring a DLI Job Bucket <dli_01_0536>`.
-  You have submitted a service ticket to request the whitelisting of the feature that allows writing job results to buckets.
-  The user who executes jobs has been granted read and write permissions either on the job bucket or on the **jobs/result** path of the job bucket.

Exporting Job Results to a Specified Location in Another Bucket
---------------------------------------------------------------

In addition to storing job results in the default bucket, you can also export them to a specified location in another bucket, increasing the flexibility of job result management and making it easier to organize and manage them.

On the console, you can only view a maximum of 1,000 job results. To view additional results, you can export them to an OBS path. The procedure is as follows:

You can export job results on either the **SQL Jobs** or the **SQL Editor** page.

-  **SQL Jobs** page: In the navigation pane on the left, choose **Job Management** > **SQL Jobs**. On the displayed page, locate the row containing a desired job, click **More** in the **Operation** column, and select **Export Result**.
-  **SQL Editor** page: In the navigation pane on the left, choose **SQL Editor**. On the displayed page, once query statements are successfully executed, click |image1| next to the **View Result** tab to export job results.

.. note::

   -  If there are no numerical columns in the query results, job results cannot be exported.
   -  Ensure that the user who exports job results has the read and write permissions on the OBS bucket.

.. table:: **Table 1** Parameters

   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                                                                                                                          |
   +=======================+=======================+======================================================================================================================================================================================================================================================================+
   | Data Format           | Yes                   | Choose a data format for the job results you want to export. The options include **json** and **csv**.                                                                                                                                                               |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Queues                | Yes                   | Select the queue where the job is executed. SQL jobs can only be executed on SQL queues.                                                                                                                                                                             |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Compression Format    | No                    | Compression format of the data to be exported. The options are:                                                                                                                                                                                                      |
   |                       |                       |                                                                                                                                                                                                                                                                      |
   |                       |                       | -  **none**                                                                                                                                                                                                                                                          |
   |                       |                       | -  **bzip2**                                                                                                                                                                                                                                                         |
   |                       |                       | -  **deflate**                                                                                                                                                                                                                                                       |
   |                       |                       | -  **gzip**                                                                                                                                                                                                                                                          |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Storage Path          | Yes                   | Path in an OBS bucket where the job results are exported                                                                                                                                                                                                             |
   |                       |                       |                                                                                                                                                                                                                                                                      |
   |                       |                       | -  If **Export Mode** is set to **New OBS directory**, then                                                                                                                                                                                                          |
   |                       |                       |                                                                                                                                                                                                                                                                      |
   |                       |                       |    You need to manually enter a directory name and ensure that the directory name does not exist. Otherwise, the system returns an error message and the export operation cannot be performed.                                                                       |
   |                       |                       |                                                                                                                                                                                                                                                                      |
   |                       |                       |    .. note::                                                                                                                                                                                                                                                         |
   |                       |                       |                                                                                                                                                                                                                                                                      |
   |                       |                       |       The folder name cannot contain special characters (``\/:*?"<>|``) and cannot start or end with a period (.).                                                                                                                                                   |
   |                       |                       |                                                                                                                                                                                                                                                                      |
   |                       |                       |    For example, after selecting the storage path **obs://bucket/src1/**, you need to manually enter a directory name to change the path to **obs://bucket/src1/src2/** and ensure that the **src2** directory name does not exist under **src1**.                    |
   |                       |                       |                                                                                                                                                                                                                                                                      |
   |                       |                       |    The job result export path is **obs://bucket/src1/src2/test.csv**.                                                                                                                                                                                                |
   |                       |                       |                                                                                                                                                                                                                                                                      |
   |                       |                       | -  If **Export Mode** is set to **Existing OBS directory (Overwritten)**, then                                                                                                                                                                                       |
   |                       |                       |                                                                                                                                                                                                                                                                      |
   |                       |                       |    After selecting a bucket path, the job results are exported to that path. If there are files with the same name, they will be automatically overwritten.                                                                                                          |
   |                       |                       |                                                                                                                                                                                                                                                                      |
   |                       |                       |    For example, if you select **obs://bucket/src1/** as the bucket path, then                                                                                                                                                                                        |
   |                       |                       |                                                                                                                                                                                                                                                                      |
   |                       |                       |    The job result export path is **obs://bucket/src1/test.csv**.                                                                                                                                                                                                     |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Export Mode           | Yes                   | -  **New OBS directory**                                                                                                                                                                                                                                             |
   |                       |                       |                                                                                                                                                                                                                                                                      |
   |                       |                       |    If you select this mode, a new folder path is created and the job results are saved to this path. This mode is used when you need to save exported results to a new location, making it easier to manage and track job results.                                   |
   |                       |                       |                                                                                                                                                                                                                                                                      |
   |                       |                       |    If you select this option, you must manually enter an export directory in **Storage Path** and ensure that the directory must not exist. If the directory already exists, the system displays an error message and the export operation cannot be performed.      |
   |                       |                       |                                                                                                                                                                                                                                                                      |
   |                       |                       | -  **Existing OBS directory (Overwritten)**: When exporting job results, you can choose an existing file path as the output directory. If there is a file with the same name in that path, it will be automatically overwritten by the new exported job result file. |
   |                       |                       |                                                                                                                                                                                                                                                                      |
   |                       |                       |    This mode is used when you only need to save a single job result file in the same path, and you do not need to keep old job results.                                                                                                                              |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Number of Results     | No                    | Number of results to be exported                                                                                                                                                                                                                                     |
   |                       |                       |                                                                                                                                                                                                                                                                      |
   |                       |                       | If you do not specify or set it to **0**, all results will be exported.                                                                                                                                                                                              |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Table Header          | No                    | Whether the job results to be exported contain table headers                                                                                                                                                                                                         |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Exporting Job Results to a Local Host
-------------------------------------

You can download the results of asynchronous DDL and QUERY statements to a local directory. By default, you can download a maximum of 1,000 data records to a local host.

The procedure is as follows:

#. Locate the row containing a desired job whose asynchronous DDL or QUERY statement has been successfully executed, click **More** in the **Operation** column, and select **Submit Download Request**. In the displayed dialog box, click **OK**. After a few seconds, the **Submit Download Request** button would change to **Download**.
#. Click **Download** to download the results to your local host.

.. |image1| image:: /_static/images/en-us_image_0000001897113369.png
