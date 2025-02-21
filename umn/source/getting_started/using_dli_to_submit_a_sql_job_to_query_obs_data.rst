:original_name: dli_01_0002.html

.. _dli_01_0002:

Using DLI to Submit a SQL Job to Query OBS Data
===============================================

Scenario
--------

DLI can query data stored in OBS. This section describes how to use DLI to submit a SQL job to query OBS data.

To illustrate, create a new file called **sampledata.csv** and upload it to an OBS bucket. Then, create an elastic resource pool, create queues within it, and use DLI to create a database and table. Finally, use DLI's SQL editor to query 1,000 data records from the table.

Procedure
---------

:ref:`Table 1 <dli_01_0002__table1478217572316>` shows the process for submitting a SQL job to query OBS data.

.. _dli_01_0002__table1478217572316:

.. table:: **Table 1** Procedure for using DLI to submit a SQL job to query OBS data

   +---------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Procedure                                                                                                     | Description                                                                                                                                                 |
   +===============================================================================================================+=============================================================================================================================================================+
   | :ref:`Step 1: Upload Data to OBS <dli_01_0002__section61379418181550>`                                        | Upload data files to OBS.                                                                                                                                   |
   +---------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`Step 2: Create an Elastic Resource Pool and Add Queues to the Pool <dli_01_0002__section1573781010172>` | Create compute resources required for submitting jobs.                                                                                                      |
   +---------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`Step 3: Create a Database <dli_01_0002__section21433273112656>`                                         | DLI metadata forms the foundation for SQL job development. Before executing a job, you need to define databases and tables based on your business scenario. |
   +---------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`Step 4: Create a Table <dli_01_0002__section21590507141153>`                                            | Use the sample data stored in OBS to create tables in the db1 database.                                                                                     |
   +---------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`Step 5: Query Data <dli_01_0002__section37788816112733>`                                                | Use standard SQL statements to query and analyze data.                                                                                                      |
   +---------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_01_0002__section61379418181550:

Step 1: Upload Data to OBS
--------------------------

Upload data files to OBS.

#. Log in to the OBS management console.

#. Create a bucket. In this example, the bucket name is **obs1**.

   a. Click **Create Bucket** in the upper right corner.
   b. On the displayed **Create Bucket** page, enter the **Bucket Name**. Retain the default values for other parameters or adjust them as needed.

      .. note::

         Select a region that matches the location of the DLI console.

   c. Click **Create Now**.

#. Click **obs1** to access its **Objects** tab page.

#. Click **Upload Object**. In the displayed dialog box, drag a desired file or folder, for example, **sampledata.csv** to the **Upload Object** area. Then, click **Upload**.

   You can create a **sampledata.txt** file, copy the following content separated by commas (,), and save the file as **sampledata.csv**.

   .. code-block::

      product_id,product_name
      113,office_13
      22,book_2
      29,book_9

   After the file is uploaded successfully, the file path is **obs://obs1/sampledata.csv**.

   For more operations on the OBS console, see the *Object Storage Service User Guide*.

.. _dli_01_0002__section1573781010172:

Step 2: Create an Elastic Resource Pool and Add Queues to the Pool
------------------------------------------------------------------

In this example, the elastic resource pool **dli_resource_pool** and queue **dli_queue_01** are created.

#. Log in to the DLI management console.

#. In the navigation pane on the left, choose **Resources** > **Resource Pool**.

#. On the displayed page, click **Buy Resource Pool** in the upper right corner.

#. On the displayed page, set the parameters.

   :ref:`Table 2 <dli_01_0002__table67098261452>` describes the parameters.

   .. _dli_01_0002__table67098261452:

   .. table:: **Table 2** Parameters

      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | Parameter          | Description                                                                                                                                                                                             | Example Value     |
      +====================+=========================================================================================================================================================================================================+===================+
      | Region             | Select a region where you want to buy the elastic resource pool.                                                                                                                                        | \_                |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | Project            | Project uniquely preset by the system for each region                                                                                                                                                   | Default           |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | Name               | Name of the elastic resource pool                                                                                                                                                                       | dli_resource_pool |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | Specifications     | Specifications of the elastic resource pool                                                                                                                                                             | Standard          |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | CU Range           | The maximum and minimum CUs allowed for the elastic resource pool                                                                                                                                       | 64-64             |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | CIDR Block         | CIDR block the elastic resource pool belongs to. If you use an enhanced datasource connection, this CIDR block cannot overlap that of the data source. **Once set, this CIDR block cannot be changed.** | 172.16.0.0/19     |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | Enterprise Project | Select an enterprise project for the elastic resource pool.                                                                                                                                             | default           |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+

#. Click **Buy**.

#. Click **Submit**.

#. In the elastic resource pool list, locate the pool you just created and click **Add Queue** in the **Operation** column.

#. Set the basic parameters listed below.

   .. table:: **Table 3** Basic parameters for adding a queue

      +-----------------------+--------------------------------------------------------------------+-----------------------+
      | Parameter             | Description                                                        | Example Value         |
      +=======================+====================================================================+=======================+
      | Name                  | Name of the queue to add                                           | dli_queue_01          |
      +-----------------------+--------------------------------------------------------------------+-----------------------+
      | Type                  | Type of the queue                                                  | \_                    |
      |                       |                                                                    |                       |
      |                       | -  To execute SQL jobs, select **For SQL**.                        |                       |
      |                       | -  To execute Flink or Spark jobs, select **For general purpose**. |                       |
      +-----------------------+--------------------------------------------------------------------+-----------------------+
      | Enterprise Project    | Select an enterprise project.                                      | default               |
      +-----------------------+--------------------------------------------------------------------+-----------------------+

#. Click **Next** and configure scaling policies for the queue.

   Click **Create** to add a scaling policy with varying priority, period, minimum CUs, and maximum CUs.

   .. table:: **Table 4** Scaling policy parameters

      +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter             | Description                                                                                                                                                                                                          | Example Value         |
      +=======================+======================================================================================================================================================================================================================+=======================+
      | Priority              | Priority of the scaling policy in the current elastic resource pool. A larger value indicates a higher priority. In this example, only one scaling policy is configured, so its priority is set to **1** by default. | 1                     |
      +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Period                | The first scaling policy is the default policy, and its **Period** parameter configuration cannot be deleted or modified.                                                                                            | 00-24                 |
      |                       |                                                                                                                                                                                                                      |                       |
      |                       | The period for the scaling policy is from 00 to 24.                                                                                                                                                                  |                       |
      +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Min CU                | Minimum number of CUs allowed by the scaling policy                                                                                                                                                                  | 16                    |
      +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Max CU                | Maximum number of CUs allowed by the scaling policy                                                                                                                                                                  | 64                    |
      +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

#. Click **OK**.

.. _dli_01_0002__section21433273112656:

Step 3: Create a Database
-------------------------

Before querying data, create a database, for example, **db1**.

.. note::

   The **default** database is a built-in database. You cannot create the **default**. database.

#. In the left navigation pane of the DLI management console, choose **SQL Editor**.

#. In the editing window on the right of the **SQL Editor** page, enter the following SQL statement and click **Execute**. Read and agree to the privacy agreement, and click **OK**.

   .. code-block::

      create database db1;

   After the database is successfully created, click |image1| in the middle pane to refresh the database list. The new database **db1** is displayed in the list.

.. _dli_01_0002__section21590507141153:

Step 4: Create a Table
----------------------

After database **db1** is created, create a table (for example, **table1**) containing data in the sample file **obs://obs1/sampledata.csv** stored on OBS in **db1**.

#. In the SQL editing window of the **SQL Editor** page, select the **default** queue and database **db1**.

#. Enter the following SQL statement in the job editor window and click **Execute**:

   .. code-block::

      create table table1 (product_id int, product_name string) using csv options (path 'obs://obs1');

   When creating a table, you only need to specify the OBS storage path where the data file is located, without specifying the file name at the end of the directory.

   After the table is successfully created, click the **Databases** tab then **db1**. The created table **table1** is displayed in the table list.

.. _dli_01_0002__section37788816112733:

Step 5: Query Data
------------------

After performing the preceding steps, you can start querying data.

#. In the **Table** tab on the **SQL Editor** page, double-click the created table **table1**. The SQL statement is automatically displayed in the SQL job editing window in the right pane. Run following statement to query 1,000 records in the **table1** table:

   .. code-block::

      select * from db1.table1 limit 1000;

#. Click **Execute**. The system starts the query.

   After the SQL statement is successfully executed or fails to be executed, you can view the query result on the **View Result** tab under the SQL job editing window.

.. |image1| image:: /_static/images/en-us_image_0000001992752425.png
