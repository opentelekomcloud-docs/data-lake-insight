:original_name: dli_01_0515.html

.. _dli_01_0515:

Creating an Elastic Resource Pool and Running a Job
===================================================

This section walks you through the procedure of adding a queue to an elastic resource pool and binding an enhanced datasource connection to the elastic resource pool.


.. figure:: /_static/images/en-us_image_0000001309847545.png
   :alt: **Figure 1** Process of creating an elastic resource pool

   **Figure 1** Process of creating an elastic resource pool

.. table:: **Table 1** Procedure

   +------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+
   | Step                                                 | Description                                                                                                                           | Reference                                                       |
   +======================================================+=======================================================================================================================================+=================================================================+
   | Create an elastic resource pool                      | Create an elastic resource pool and configure basic information, such as the billing mode, CU range, and CIDR block.                  | :ref:`Creating an Elastic Resource Pool <dli_01_0505>`          |
   +------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+
   | Add a queue to the elastic resource pool             | Add the queue where your jobs will run on to the elastic resource pool. The operations are as follows:                                | :ref:`Adding a Queue <dli_01_0509>`                             |
   |                                                      |                                                                                                                                       |                                                                 |
   |                                                      | #. Set basic information about the queue, such as the name and type.                                                                  | :ref:`Managing Queues <dli_01_0506>`                            |
   |                                                      | #. Configure the scaling policy of the queue, including the priority, period, and the maximum and minimum CUs allowed for scaling.    |                                                                 |
   +------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+
   | (Optional) Create an enhanced datasource connection. | If a job needs to access data from other data sources, for example, GaussDB(DWS) and RDS, you need to create a datasource connection. | :ref:`Creating an Enhanced Datasource Connection <dli_01_0006>` |
   |                                                      |                                                                                                                                       |                                                                 |
   |                                                      | The created datasource connection must be bound to the elastic resource pool.                                                         |                                                                 |
   +------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+
   | Run a job.                                           | Create and submit the job as you need.                                                                                                | :ref:`SQL Job Management <dli_01_0017>`                         |
   |                                                      |                                                                                                                                       |                                                                 |
   |                                                      |                                                                                                                                       | :ref:`Overview <dli_01_0403>`                                   |
   |                                                      |                                                                                                                                       |                                                                 |
   |                                                      |                                                                                                                                       | :ref:`Creating a Spark Job <dli_01_0384>`                       |
   +------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+

.. _dli_01_0515__section20375737115219:

Step 1: Create an Elastic Resource Pool
---------------------------------------

#. Log in to the DLI management console. In the navigation pane on the left, choose **Resources** > **Resource Pool**.

#. On the displayed **Resource Pool** page, click **Buy Resource Pool** in the upper right corner.

#. On the displayed page, set the following parameters:

   -  **Name**: Enter the name of the elastic resource pool. For example, **pool_test**.
   -  **CU range**: Minimum and maximum CUs of the elastic resource pool.
   -  **CIDR Block**: Network segment of the elastic resource pool. For example, **172.16.0.0/18**.
   -  Set other parameters as required.

   For details about how to create an elastic resource pool, see :ref:`Creating an Elastic Resource Pool <dli_01_0505>`.

#. Click **Buy**. Confirm the configuration and click **Pay**.

#. Go to the **Resource Pool** page to view the creation status. If the status is **Available**, the elastic resource pool is ready for use.

.. _dli_01_0515__section117761251115:

Step 2: Add a Queue to the Elastic Resource Pool
------------------------------------------------

#. In the **Operation** column of the created elastic resource pool, click **Add Queue**.
#. Specify the basic information about the queue. The configuration parameters are as follows:

   -  **Name**: Queue name

   -  **Type**: Queue type In this example, select **For general purpose**.

      **For SQL**: The queue is used to run Spark SQL jobs.

      **For general purpose**: The queue is used to run Flink and Spark Jar jobs.

   -  Set other parameters as required.

#. Click **Next**. On the displayed page, set **Min CU** to **64** and **Max CU** to **64**.
#. Click **OK**. The queue is added.

(Optional) Step 3: Create an Enhanced Datasource Connection
-----------------------------------------------------------

In this example, a datasource connection is required to connect to RDS. You need to create a datasource connection. If your job does not need to connect to an external data source, skip this step.

#. Log in to the RDS console and create an RDS DB instance. For details, see . Log in to the RDS instance, create a database and name it **test2**.

#. Locate the row that contains the **test2** database, click **Query SQL Statements** in the **Operation** column. On the displayed page, enter the following statement to create table **tabletest2**. Click **Execute SQL**. The table creation statement is as follows:

   .. code-block::

      CREATE TABLE `tabletest2` (
          `id` int(11) unsigned,
          `name` VARCHAR(32)
      )   ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4;

#. On the RDS console, choose **Instances** form the navigation pane. Click the name of a created RDS DB instance to view its basic information.

#. .. _dli_01_0515__li19197133109:

   In the **Connection Information** pane, obtain the floating IP address, database port, VPC, and subnet.

#. Click the security group name. In the **Inbound Rules** tab, add a rule to allow access from the CIDR block of the elastic resource pool. For example, if the CIDR block of the elastic resource pool is **172.16.0.0/18** and the database port is **3306**, set the rule **Priority** to **1**, **Action** to **Allow**, **Protocol** to **TCP** and **Port** to **3306**, Type to **IPv4**, and **Source** to **172.16.0.0/18**.

   Click **OK**. The security group rule is added.

#. Log in to the DLI management console. In the navigation pane on the left, choose **Datasource Connections**. On the displayed page, click **Create** in the **Enhanced** tab.

#. In the displayed dialog box, set the following parameters:

   -  **Connection Name**: Name of the enhanced datasource connection
   -  **Resource Pool**: Select the elastic resource pool created in :ref:`Step 1: Create an Elastic Resource Pool <dli_01_0515__section20375737115219>`.

      .. note::

         If you cannot decide the elastic resource pool in this step, you can skip this parameter, go to the **Enhanced** tab, and click **More** > **Bind Resource Pool** in the **Operation** column of the row that contains this datasource connection after it is created.

   -  **VPC**: Select the VPC of the RDS DB instance obtained in :ref:`4 <dli_01_0515__li19197133109>`.
   -  **Subnet**: Select the subnet of the RDS DB instance obtained in :ref:`4 <dli_01_0515__li19197133109>`.
   -  Set other parameters as you need.

   Click **OK**. Click the name of the created datasource connection to view its status. You can perform subsequent steps only after the connection status changes to **Active**.

#. Click **Resources** > **Queue Management**, select the target queue, for example, **general_test**. In the **Operation** column, click **More** and select **Test Address Connectivity**.

#. In the displayed dialog box, enter *Floating IP address*\ **:**\ *Database port* of the RDS database in the **Address** box and click **Test** to check whether the database is reachable.

Step 4: Run a Job
-----------------

Run a Flink SQL jab on a queue in an elastic resource pool.

#. On the DLI management console, choose **Job Management** > **Flink Jobs**. On the **Flink Jobs** page, click **Create Job**.
#. In the **Create Job** dialog box, set **Type** to **Flink SQL** and **Name** to **testFlinkSqlJob**. Click **OK**.
#. On the job editing page, set the following parameters:

   -  **Queue**: Select the **general_test** queue added to the elastic resource pool in :ref:`Step 2: Add a Queue to the Elastic Resource Pool <dli_01_0515__section117761251115>`.

   -  **Save Job Log**: Enable this function.

   -  **OBS Bucket**: Select an OBS bucket for storing job logs and grant access permissions of the OBS bucket as prompted.

   -  **Enable Checkpointing**: Enable.

   -  Enter the SQL statement in the editing pane. The following is an example. Modify the parameters in bold as you need.

      .. code-block::

         CREATE SINK STREAM car_info (id INT, name STRING) WITH (
           type = "rds",
           region = "", /* Change the value to the current region ID. */
            'pwd_auth_name'="xxxxx", // Name of the datasource authentication of the password type created on DLI. If datasource authentication is used, you do not need to set the username and password for the job.
         db_url = "mysql://192.168.x.x:3306/test2", /* The format is mysql://floating IP address:port number of the RDS database/database name. */
         table_name = "tabletest2" /* Table name in RDS database */
         );
         INSERT INTO
           car_info
         SELECT
           13,
           'abc';

#. Click **Check Semantic** and ensure that the SQL statement passes the check. Click **Save**. Click **Start**, confirm the job parameters, and click **Start Now** to execute the job.
#. Wait until the job is complete. The job status changes to **Completed**.
#. Log in to the RDS console, click the name of the RDS DB instance. On the displayed page, click the name of the created database, for example, **test2**, and click **Query SQL Statements** in the **Operation** column of the row that containing the **tabletest2** table.
#. On the displayed page, click **Execute SQL**. Check whether data has been written into the RDS table.
