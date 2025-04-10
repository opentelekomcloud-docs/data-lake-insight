:original_name: dli_01_0320.html

.. _dli_01_0320:

Creating and Submitting a SQL Job
=================================

Introduction
------------

The SQL Editor allows you to execute data query operations using SQL statements.

It supports SQL:2003 and is compatible with Spark SQL.

To access the SQL editor, choose **SQL Editor** in the left navigation pane of the DLI console, or click **Create Job** in the upper right corner of the **Job Management** > **SQL Jobs** page.

This section describes how to create and submit a SQL job using the DLI SQL editor.

Notes
-----

-  If you access the SQL editor for the first time, the system prompts you to set a bucket for DLI jobs. The created bucket is used to store temporary data generated by DLI, such as job logs.

   You cannot view job logs if you choose not to create the bucket. The bucket name will be set by the system.

   On the OBS console, you can configure lifecycle rules for a bucket to periodically delete objects in it or change object storage classes.

-  SQL statements can be executed in batches on the SQL editor page.

-  Commonly used syntax in the job editing window is highlighted in different colors.
-  Both single-line comment and multi-line comment are allowed. Use two consecutive hyphens (--) in each line to comment your statements.

Creating and Submitting a SQL Job Using the SQL Editor
------------------------------------------------------

#. Log in to the DLI management console. In the navigation pane on the left, choose **SQL Editor**.

   .. note::

      On the SQL editor page, the system prompts you to create an OBS bucket to store temporary data generated by DLI jobs. In the **Set Job Bucket** dialog box, click **Setting**. On the page displayed, click the edit button in the upper right corner of the job bucket card. In the displayed **Set Job Bucket** dialog box, enter the job bucket path and click **OK**.

#. Above the SQL job editing window, set the parameters required for running a SQL job, such as the queue and database. For how to set the parameters, refer to :ref:`Table 1 <dli_01_0320__table151023712614>`.

   .. _dli_01_0320__table151023712614:

   .. table:: **Table 1** Setting SQL job parameters

      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Button & Drop-Down List           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      +===================================+=========================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
      | Queues                            | Select a queue from the drop-down list box. If there is no queue available, the **default** queue is displayed. However, this queue is shared among all users for experience only. This may lead to resource contention, preventing you from obtaining the required resources for your jobs. So, you are advised to create your own queue for executing your jobs. For how to create a queue, see :ref:`Creating an Elastic Resource Pool and Creating Queues Within It <dli_01_0505>`. |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
      |                                   | SQL jobs can only be executed on SQL queues.                                                                                                                                                                                                                                                                                                                                                                                                                                            |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Databases                         | Select a database from the drop-down list box. If no database is available, the **default** database is displayed. For how to create a database, see :ref:`Creating a Database and Table on the DLI Console <dli_01_0005>`.                                                                                                                                                                                                                                                             |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
      |                                   | .. note::                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
      |                                   |    If you have specified a database where tables are located in SQL statements, the database you choose here does not apply.                                                                                                                                                                                                                                                                                                                                                            |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Settings                          | Add parameters and tags.                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
      |                                   | **Parameter Settings**: Set parameters in key/value format for SQL jobs.                                                                                                                                                                                                                                                                                                                                                                                                                |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
      |                                   | **Tags**: Assign key-value pairs as tags to a SQL job.                                                                                                                                                                                                                                                                                                                                                                                                                                  |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Create a database and a table.

   Create them in advance by referring to :ref:`Creating a Database and Table on the DLI Console <dli_01_0005>`. For example, create a table named **qw**.

#. In the SQL job editing window, enter the following SQL statement:

   ::

      SELECT * FROM qw.qw LIMIT 10;

   Alternatively, you can double-click the table name **qw**. The query statement is automatically entered in the SQL job editing window.

   DLI offers a range of SQL templates that come with use cases, code examples, and usage guides. You can also use these templates to quickly implement your service logic. For more information about templates, see :ref:`Creating a SQL Job Template <dli_01_0021>`.

#. On top of the editing window, click **More** > **Verify Syntax** to check whether the SQL statement is correct.

   a. If the verification fails, check the SQL statement syntax by referring to *Data Lake Insight SQL Syntax Reference*.
   b. If the syntax verification is successful, click **Execute**. Read and agree to the privacy agreement. Click **OK** to execute the SQL statement.

   Once successfully executed, you can check the execution result on the **View Result** tab below the SQL job editing window.

#. View job execution results.

   On the **View Result** tab, click |image1| to display execution results in a chart. Click |image2| to switch back to the table view.

   You can view a maximum of 1,000 data records on this **View Result** tab. To view more or full data, click |image3| to export the data to OBS.

   .. note::

      -  If no column of the numeric type is displayed in the execution result, the result cannot be represented in charts.
      -  You can view the data in a bar chart, line chart, or fan chart.
      -  In the bar chart and line chart, the X axis can be any column, while the Y axis can only be columns of the numeric type. The fan chart displays the corresponding legends and indicators.

Functions of SQL Editor
-----------------------

-  **Parameter settings for SQL jobs**

   Click **Settings** in the upper right corner of the **SQL Editor** page. You can set parameters and tags for the SQL job.

   -  **Parameter Settings**: Assign key-value pairs as parameter settings.
   -  **Tags**: Assign key-value pairs as tags to a SQL job.

   .. table:: **Table 2** Parameters for SQL job running

      +---------------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                                   | Default Value         | Description                                                                                                                                                                                                                                                                                                                                      |
      +=============================================+=======================+==================================================================================================================================================================================================================================================================================================================================================+
      | spark.sql.files.maxRecordsPerFile           | 0                     | Maximum number of records to be written into a single file. If the value is zero or negative, there is no limit.                                                                                                                                                                                                                                 |
      +---------------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | spark.sql.autoBroadcastJoinThreshold        | 209715200             | Maximum size, in bytes, of the table that displays all working nodes when a connection is executed. You can set this parameter to **-1** to disable the display.                                                                                                                                                                                 |
      |                                             |                       |                                                                                                                                                                                                                                                                                                                                                  |
      |                                             |                       | .. note::                                                                                                                                                                                                                                                                                                                                        |
      |                                             |                       |                                                                                                                                                                                                                                                                                                                                                  |
      |                                             |                       |    Currently, only configuration units that store tables analyzed using the **ANALYZE TABLE COMPUTE statistics noscan** command and file-based data source tables that calculate statistics directly from data files are supported.                                                                                                              |
      +---------------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | spark.sql.shuffle.partitions                | 200                   | Default number of partitions used to filter data for join or aggregation.                                                                                                                                                                                                                                                                        |
      +---------------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | spark.sql.dynamicPartitionOverwrite.enabled | false                 | When set to **false**, DLI will delete all partitions that meet the conditions before overwriting them. For example, if there is a partition named **2021-01** in a partitioned table and you use the **INSERT OVERWRITE** statement to write data to the **2021-02** partition, the data in the **2021-01** partition will also be overwritten. |
      |                                             |                       |                                                                                                                                                                                                                                                                                                                                                  |
      |                                             |                       | When set to **true**, DLI will not delete partitions in advance, but will overwrite partitions with data written during runtime.                                                                                                                                                                                                                 |
      +---------------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | spark.sql.files.maxPartitionBytes           | 134217728             | Maximum number of bytes to be packed into a single partition when a file is read.                                                                                                                                                                                                                                                                |
      +---------------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | spark.sql.badRecordsPath                    | ``-``                 | Path of bad records.                                                                                                                                                                                                                                                                                                                             |
      +---------------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | dli.sql.sqlasync.enabled                    | true                  | Whether DDL and DCL statements are executed asynchronously. The value **true** indicates that asynchronous execution is enabled.                                                                                                                                                                                                                 |
      +---------------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | dli.sql.job.timeout                         | ``-``                 | Job running timeout interval, in seconds. If the job times out, it will be canceled.                                                                                                                                                                                                                                                             |
      +---------------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

-  **Switching to the SparkUI page to view the SQL statement execution process**

   The SQL editor allows you to switch to the SparkUI to view the SQL statement execution process.

   -  You can view only the latest 100 job records on DLI's SparkUI.
   -  If a job is running on the **default** queue or is a synchronization one, you cannot switch to the SparkUI to view the SQL statement execution process.

   .. note::

      When you execute a job on a created queue, the cluster is restarted. It takes about 10 minutes. If you click **SparkUI** before the cluster is created, an empty **projectID** will be cached. The SparkUI page cannot be displayed. You are advised to use a dedicated queue so that the cluster will not be released. Alternatively, wait for a while after the job is submitted (the cluster is created), and then check **SparkUI**.

-  **Archiving SQL run logs**

   On the **Executed Queries (Last Day)** tab of the **SQL Editor** page, click **More** and select **View Log** in the **Operation** column of the SQL job. The system automatically switches to the OBS path where logs are stored. You can download logs as needed.

   .. note::

      The **View Log** button is not available for synchronization jobs and jobs running on the **default** queue.

-  **SQL Editor shortcuts**

   .. table:: **Table 3** Keyboard shortcuts

      +-------------+---------------------------------------------------------------------------------------------------------------------------------------+
      | Shortcut    | Description                                                                                                                           |
      +=============+=======================================================================================================================================+
      | Ctrl+Enter  | Execute SQL statements. You can run SQL statements by pressing **Ctrl+R** or **Ctrl + Enter** on the keyboard.                        |
      +-------------+---------------------------------------------------------------------------------------------------------------------------------------+
      | Ctrl+F      | Search for SQL statements. You can press Ctrl+F to search for a required SQL statement.                                               |
      +-------------+---------------------------------------------------------------------------------------------------------------------------------------+
      | Shift+Alt+F | Format SQL statements. You can press **Shift + Alt + F** to format a SQL statement.                                                   |
      +-------------+---------------------------------------------------------------------------------------------------------------------------------------+
      | Ctrl+Q      | Syntax verification. You can press **Ctrl + Q** to verify the syntax of SQL statements.                                               |
      +-------------+---------------------------------------------------------------------------------------------------------------------------------------+
      | F11         | Full screen. You can press **F11** to display the SQL Job Editor window in full screen. Press **F11** again to leave the full screen. |
      +-------------+---------------------------------------------------------------------------------------------------------------------------------------+

.. |image1| image:: /_static/images/en-us_image_0000002031969193.png
.. |image2| image:: /_static/images/en-us_image_0000002031849645.png
.. |image3| image:: /_static/images/en-us_image_0000001209489750.png
