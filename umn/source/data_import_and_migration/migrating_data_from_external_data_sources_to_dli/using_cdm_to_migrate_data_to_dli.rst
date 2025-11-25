:original_name: dli_01_0633.html

.. _dli_01_0633:

Using CDM to Migrate Data to DLI
================================

On its GUI, CDM enables you to create data migration tasks from multiple data sources to a data lake.

This section describes how to use CDM to migrate data from data sources to DLI.


.. figure:: /_static/images/en-us_image_0000002295795461.png
   :alt: **Figure 1** Process of migrating data to DLI using CDM

   **Figure 1** Process of migrating data to DLI using CDM

.. _dli_01_0633__section033932654416:

Step 1: Create a CDM Cluster
----------------------------

A CDM cluster is used to execute data migration jobs that migrate data from data sources to DLI.

#. Log in to the DataArts Studio management console and access the page for creating a CDM cluster.

#. Set cluster parameters.

   -  You are advised to set the region, VPC, subnet, security group, and enterprise project to be the same as those of the data source and DLI.
   -  Once a cluster is created, its specifications cannot be modified. If you need to use higher specifications, you will need to create a cluster.

   For more information about CDM cluster parameters, see *DataArts Studio User Guide*.

#. Click **Buy Now**.

#. Confirm the settings and click **Submit**. The system starts to create a CDM cluster. You can check the creation progress on the **Cluster Management** page.

Step 2: Create a Data Connection Between the Data Source and CDM
----------------------------------------------------------------

This step uses the MySQL data source as an example to describe how to create a data connection between the data source and CDM.

#. In the navigation pane on the left of the CDM console, choose **Cluster Management**. Locate the **cdm-aff1** cluster created in :ref:`Step 1: Create a CDM Cluster <dli_01_0633__section033932654416>`.
#. Click **Job Management** in the **Operation** column.
#. Click the **Link Management** tab then **Create Link**.

4. Select **RDS for MySQL** and click **Next** to set the link parameters.

   Click **Show Advanced Attributes** to show optional parameters.

   Retain the default values for the optional parameters and set the mandatory parameters based on :ref:`Table 1 <dli_01_0633__table5321744015490>`.

   .. _dli_01_0633__table5321744015490:

   .. table:: **Table 1** MySQL link parameters

      +----------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Parameter                  | Description                                                                                                                                                                                                                                                      | Example Value |
      +============================+==================================================================================================================================================================================================================================================================+===============+
      | Name                       | Enter a unique link name.                                                                                                                                                                                                                                        | mysqllink     |
      +----------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Database Server            | IP address or domain name of the MySQL database                                                                                                                                                                                                                  | ``-``         |
      +----------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Port                       | Port number of the MySQL database                                                                                                                                                                                                                                | 3306          |
      +----------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Database Name              | Name of the MySQL database                                                                                                                                                                                                                                       | sqoop         |
      +----------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Username                   | User who has the read, write, and delete permissions on the MySQL database                                                                                                                                                                                       | admin         |
      +----------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Password                   | Password of the user                                                                                                                                                                                                                                             | ``-``         |
      +----------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Use Local API              | Whether to use the local API of the database for acceleration. (The system attempts to enable the **local_infile** system variable of the MySQL database.)                                                                                                       | Yes           |
      +----------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Use Agent                  | This parameter does not need to be set as the agent function will be unavailable soon.                                                                                                                                                                           | ``-``         |
      +----------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | local_infile Character Set | When using local_infile to import data to MySQL, you can set the encoding format.                                                                                                                                                                                | utf8          |
      +----------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Driver Version             | Before connecting CDM to a relational database, you need to upload the JDK 8 .jar driver of the relational database. Download the MySQL driver 5.1.48 from https://downloads.mysql.com/archives/c-j/, obtain **mysql-connector-java-5.1.48.jar**, and upload it. | ``-``         |
      +----------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+

5. Click **Test** to check whether the parameters are correctly configured. If the test is successful, click **Save** to create the link and return to the **Links** page.

Step 3: Create a Data Connection Between CDM and DLI
----------------------------------------------------

#. In the navigation pane on the left of the CDM console, choose **Cluster Management**. Locate the **cdm-aff1** cluster created in :ref:`Step 1: Create a CDM Cluster <dli_01_0633__section033932654416>`.
#. Click **Job Management** in the **Operation** column.
#. Click the **Link Management** tab then **Create Link**.

4. Select **Data Lake Insight** for **Data Warehouse** and click **Next**. On the displayed page, set DLI link parameters.

   Click **Show Advanced Attributes** to show optional parameters. Retain the default values. :ref:`Table 2 <dli_01_0633__table22075105144748>` describes the mandatory parameters.

   .. _dli_01_0633__table22075105144748:

   .. table:: **Table 2** DLI link parameters

      +-----------------------+--------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter             | Description                                                                                                        | Example Value         |
      +=======================+====================================================================================================================+=======================+
      | Name                  | Link name, which should be defined based on the data source type, so it is easier to remember what the link is for | dlilink               |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Access Key            | AK/SK required for authentication during access to the DLI database.                                               | ``-``                 |
      |                       |                                                                                                                    |                       |
      |                       | You need to create an access key for the current account and obtain an AK/SK pair.                                 |                       |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Secret Key            |                                                                                                                    | ``-``                 |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Project ID            | Project ID of the region where DLI is                                                                              | ``-``                 |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------+-----------------------+

5. Click **Test** to check whether the parameters are correctly configured. If the test is successful, click **Save** to create the link and return to the **Links** page.

.. _dli_01_0633__section1458245421018:

Step 4: Create a Data Migration Job on CDM
------------------------------------------

After establishing the data connection between the data source and CDM, as well as between CDM and DLI, you need to create a data migration job to migrate the data from the data source to DLI.

#. On the **Cluster Management** page, locate the **cdm-aff1** cluster created in :ref:`Step 1: Create a CDM Cluster <dli_01_0633__section033932654416>`.

#. Click **Job Management** in the **Operation** column.

#. Click the **Table/File Migration** tab, click **Create Job**, and configure basic job information.

   -  **Job Name**: Enter a unique job name, for example, **mysql2dli**.
   -  Source job parameters

      -  **Source Link Name**: Select the MySQL link **mysqllink**.
      -  **Use SQL**: Select **No**.
      -  **Schema/Tablespace**: Select the MySQL database from which the table is to be exported.
      -  **Table Name**: Select the table from which data is to be exported.
      -  Retain the default values for other parameters.

   -  Destination job parameters

      -  **Destination Link Name**: Select the DLI link **dlilink**.
      -  **Schema/Tablespace**: Select the schema to which data is to be imported.
      -  **Auto Table Creation**: Select **Auto creation**. If the table specified by **Table Name** does not exist, CDM automatically creates the table in DLI.
      -  **Table Name**: Select the table to which data is to be imported.
      -  **Advanced Attributes** > **Extend Field Length**: Select **Yes**.
      -  Retain the default values for other parameters.

#. Click **Next**. The **Map Field** tab page is displayed. CDM automatically maps the fields in the source and destination data tables. You need to check if the field mapping is correct.

   -  If the mapping is incorrect, click the row where the field is located and hold down the left mouse button to drag the field to adjust the mapping.
   -  When importing data into DLI, you must manually choose the distribution columns. You are advised to select the distribution columns based on the following principles:

      a. Use the primary key as the distribution column.
      b. If multiple data segments are combined as primary keys, specify all primary keys as the distribution column.
      c. In the scenario where no primary key is available, if no distribution column is selected, DWS uses the first column as the distribution column by default. As a result, data skew risks exist.

   -  To convert the content of the source fields, perform the operations in this step. In this example, field conversion is not required.

#. Click **Next** and set task parameters. Typically, retain the default values for all parameters.

   In this step, you can configure the following optional features:

   -  **Retry Upon Failure**: If the job fails to be executed, you can determine whether to automatically retry. Retain the default value **Never**.
   -  **Group**: Select the group to which the job belongs. The default group is **DEFAULT**. On the **Job Management** page, jobs can be displayed, started, or exported by group.
   -  **Schedule Execution**: Determine whether to automatically execute the job at a scheduled time. Retain the default value **No**.
   -  **Concurrent Extractors**: Enter the number of concurrent extractors. An appropriate value improves migration efficiency. Retain the default value **1**.
   -  **Write Dirty Data**: Specify this parameter if data that fails to be processed or filtered out during job execution needs to be written to OBS for future viewing. Before writing dirty data, create an OBS link on the CDM console. Retain the default value **No**, meaning dirty data is not recorded.

#. Click **Save and Run**. CDM starts to execute the job immediately.

Step 5: View Data Migration Results
-----------------------------------

This step describes how to view a job's execution results and its historical information within the past 90 days, including the number of written rows, read rows, written bytes, written files, and log information.

-  **Viewing the status of the migration job on CDM**

   #. On the **Cluster Management** page, locate the **cdm-aff1** cluster created in :ref:`Step 1: Create a CDM Cluster <dli_01_0633__section033932654416>`.
   #. Click **Job Management** in the **Operation** column.
   #. Locate the **mysql2dli** job created in :ref:`Step 4: Create a Data Migration Job on CDM <dli_01_0633__section1458245421018>` and check its execution status. If the job status is **Succeeded**, the migration is successful.

-  **Viewing data migration results on DLI**

   #. After the CDM migration job is complete, log in to the DLI management console.

   #. In the navigation pane on the left, choose **SQL Editor**.

      Set **Engine** to **Spark**, **Queues** to the created SQL queue, and **Databases** to the created database. Run the following DLI table query statement to check whether the MySQL data has been successfully migrated to the DLI table:

      .. code-block::

         select * from tablename;
