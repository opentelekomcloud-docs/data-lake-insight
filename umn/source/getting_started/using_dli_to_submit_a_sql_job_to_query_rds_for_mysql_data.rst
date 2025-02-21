:original_name: dli_01_0020.html

.. _dli_01_0020:

Using DLI to Submit a SQL Job to Query RDS for MySQL Data
=========================================================

Scenario
--------

DLI can query data stored in RDS. This section describes how to use DLI to submit a SQL job to query RDS for MySQL data.

In this example, we will create an RDS for MySQL DB instance, create a database and table, create a DLI elastic resource pool and add a queue to it, create an enhanced datasource connection to connect the DLI elastic resource pool and the RDS for MySQL DB instance, and submit a SQL job to access the data in the RDS table.

Procedure
---------

:ref:`Table 1 <dli_01_0020__table1478217572316>` shows the process for submitting a SQL job to query RDS for MySQL data.

.. _dli_01_0020__table1478217572316:

.. table:: **Table 1** Procedure for using DLI to submit a SQL job to query RDS for MySQL data

   +---------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
   | Procedure                                                                                                     | Description                                                                                                                      |
   +===============================================================================================================+==================================================================================================================================+
   | :ref:`Step 1: Create an RDS MySQL Instance <dli_01_0020__section10891114913473>`                              | Create an RDS for MySQL instance for the example scenario.                                                                       |
   +---------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`Step 2: Create an RDS Database Table <dli_01_0020__section18912701608>`                                 | Log in to the RDS for MySQL DB instance and create a database and a table.                                                       |
   +---------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`Step 3: Create an Elastic Resource Pool and Add Queues to the Pool <dli_01_0020__section1573781010172>` | Create compute resources required for submitting jobs.                                                                           |
   +---------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`Step 4: Create an Enhanced Datasource Connection <dli_01_0020__section107452812378>`                    | Create an enhanced datasource connection to connect the DLI elastic resource pool and the RDS for MySQL DB instance.             |
   +---------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`Step 5: Create a Datasource Authentication <dli_01_0020__section1134817141019>`                         | Create a datasource authentication to store the access credentials required by DLI to read from and write to RDS for MySQL data. |
   +---------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`Step 6: Submit a SQL Job <dli_01_0020__section21590507141153>`                                          | Use standard SQL statements to query and analyze data.                                                                           |
   +---------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+

.. _dli_01_0020__section10891114913473:

Step 1: Create an RDS MySQL Instance
------------------------------------

In this example, assuming the job name is **JobSample**, RDS is used as the data source to create an RDS for MySQL DB instance.

For details, see "RDS for MySQL Getting Started" in the *Relational Database Service Getting Started*.

#. Log in to the RDS management console.

#. Select a region and a project in the upper left corner.

#. In the navigation pane on the left, choose **Instances**. On the displayed page, click **Buy DB Instance** in the upper right corner.

#. On the **Buy DB Instance** page, set instance parameters, and click **Buy**.

   Set the parameters listed below based on your service planning.

   .. table:: **Table 2** RDS for MySQL instance parameters

      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | Parameter              | Description                                                                                                                                                                                                                           | Example Value             |
      +========================+=======================================================================================================================================================================================================================================+===========================+
      | Region                 | Region where you want to create the instance                                                                                                                                                                                          | \_                        |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | DB Instance Name       | Instance name                                                                                                                                                                                                                         | rds-demo                  |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | DB Engine              | MySQL                                                                                                                                                                                                                                 | MySQL                     |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | DB Engine Version      | If you select **MySQL** for **DB Engine**, select an engine version that best suits your service needs. You are advised to select the latest available version for more stable performance, higher security, and greater reliability. | 8.0                       |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | DB Instance Type       | Primary/standby mode of the instance                                                                                                                                                                                                  | Single                    |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | Storage Type           | Determines the instance read/write speed. The higher the maximum throughput, the faster the read and write speeds.                                                                                                                    | Cloud SSD                 |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | AZ                     | For a single DB instance, you only need to select a single AZ.                                                                                                                                                                        | ``-``                     |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | Time Zone              | Select a time zone based on the region you selected. You can change it after the DB instance is created.                                                                                                                              | Retain the default value. |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | Instance Class         | vCPUs and memory. These instance classes support varying number of connections and maximum IOPS.                                                                                                                                      | 2 vCPUs \| 4 GB           |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | Storage Space          | If the storage type is cloud SSD or extreme SSD, you can enable storage autoscaling. If the available storage drops to a specified threshold, autoscaling is triggered.                                                               | 40 GB                     |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | Disk Encryption        | Determine whether to enable disk encryption.                                                                                                                                                                                          | Disable                   |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | VPC                    | Select an existing VPC.                                                                                                                                                                                                               | ``-``                     |
      |                        |                                                                                                                                                                                                                                       |                           |
      |                        | For how to recreate a VPC and subnet, refer to "Creating a VPC and Subnet" in the *Virtual Private Cloud User Guide*.                                                                                                                 |                           |
      |                        |                                                                                                                                                                                                                                       |                           |
      |                        | .. note::                                                                                                                                                                                                                             |                           |
      |                        |                                                                                                                                                                                                                                       |                           |
      |                        |    In datasource scenarios, the CIDR block of the data source cannot overlap that of the elastic resource pool.                                                                                                                       |                           |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | Database Port          | Port **3306** is used by default.                                                                                                                                                                                                     | 3306                      |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | Security Group         | Enhances security by providing rules that control access to RDS from other services.                                                                                                                                                  | ``-``                     |
      |                        |                                                                                                                                                                                                                                       |                           |
      |                        | The security group where the data source is must allow access from the CIDR block of the DLI elastic resource pool.                                                                                                                   |                           |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | Password               | Set a password for logging in to the DB instance.                                                                                                                                                                                     | ``-``                     |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | Administrator          | root                                                                                                                                                                                                                                  | root                      |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | Administrator Password | Administrator password                                                                                                                                                                                                                | ``-``                     |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | Parameter Template     | A template of parameters for creating an instance. The template contains engine configuration values that are applied to one or more instances.                                                                                       | Default-MySQL-5.7         |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | Table Name             | Determines whether the table name is case-insensitive.                                                                                                                                                                                | Case insensitive          |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | Enterprise Project     | If the instance has been associated with an enterprise project, select the target project from the **Enterprise Project** drop-down list.                                                                                             | default                   |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
      | Quantity               | Number of instances to buy                                                                                                                                                                                                            | 1                         |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+

#. Click **Next**. The confirmation page is displayed.

#. Click **Submit**.

#. To view and manage the DB instance, go to the **Instance Management** page.

   During the creation process, the DB instance status is **Creating**. When the creation process is complete, the instance status will change to **Available**. You can view the detailed progress and result of the task on the **Task Center** page.

.. _dli_01_0020__section18912701608:

Step 2: Create an RDS Database Table
------------------------------------

#. Log in to the RDS management console.

#. In the upper left corner of the management console, select the target region and project.

#. On the **Instances** page, locate the DB instance you just created and record its floating IP address.

#. Locate the RDS for MySQL DB instance you created, click **More** in the **Operation** column, and select **Log In**. On the displayed page, enter the username and password for logging in to the instance and click **Test Connection**. After **Connection is successful** is displayed, click **Log In**.

#. Click **Create Database**. In the displayed dialog box, enter database name **dli_demo**. Then, click **OK**.

#. Click **SQL Query** and run the following SQL statement to create a table:

   .. code-block::

      CREATE TABLE `dli_demo`.`tabletest` (
          `id` VARCHAR(32) NOT NULL,
          `name` VARCHAR(32) NOT NULL,
          PRIMARY KEY (`id`)
      )   ENGINE = InnoDB
          DEFAULT CHARACTER SET = utf8mb4;

.. _dli_01_0020__section1573781010172:

Step 3: Create an Elastic Resource Pool and Add Queues to the Pool
------------------------------------------------------------------

To execute SQL jobs in datasource scenarios, you must use your own SQL queue as the existing **default** queue cannot be used. In this example, create an elastic resource pool named **dli_resource_pool** and a queue named **dli_queue_01**.

#. Log in to the DLI management console.

#. In the navigation pane on the left, choose **Resources** > **Resource Pool**.

#. On the displayed page, click **Buy Resource Pool** in the upper right corner.

#. On the displayed page, set the parameters.

   :ref:`Table 3 <dli_01_0020__dli_01_0002_table67098261452>` describes the parameters.

   .. _dli_01_0020__dli_01_0002_table67098261452:

   .. table:: **Table 3** Parameters

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

   .. table:: **Table 4** Basic parameters for adding a queue

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

   .. table:: **Table 5** Scaling policy parameters

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

.. _dli_01_0020__section107452812378:

Step 4: Create an Enhanced Datasource Connection
------------------------------------------------

#. Create a rule on the security group of the RDS DB instance to allow access from the CIDR block of the DLI queue.

   a. Go to the RDS management console and choose **Instances** in the navigation pane on the left. In the instance list, click the name of the RDS for MySQL DB instance you created to access its basic information page.

   b. In the navigation pane on the left, choose **Connectivity & Security**. In the **Security Group Rules** area, find the **Inbound Rules** tab and click **Add Inbound Rule**.

      For example, if the CIDR block of the queue is **172.16.0.0/19**, add the rule as follows:

      -  **Type**: Select **IPv4**.
      -  **Protocol & Port**: Select **Protocols/TCP (Custom)** as the protocol and leave the port number blank.
      -  **Source**: Select **IP Address** and enter **172.16.0.0/19**.

      Click **OK**. The security group rule is added.

#. Create an enhanced datasource connection between RDS for MySQL and DLI.

   .. note::

      The CIDR block of the elastic resource pool bound to a datasource connection cannot overlap that of the data source.

   a. Go to the DLI management console and choose **Datasource Connections** in the navigation pane on the left.
   b. On the displayed **Enhanced** tab, click **Create**. Set the following parameters:

      -  **Connection Name**: **dlirds**

      -  **Resource Pool**: Select the elastic resource pool created in :ref:`Step 3: Create an Elastic Resource Pool and Add Queues to the Pool <dli_01_0020__section1573781010172>`.

      -  **VPC**: Select the VPC where the RDS for MySQL DB instance is, that is, the VPC selected in :ref:`Step 2: Create an RDS Database Table <dli_01_0020__section18912701608>`.

      -  **Subnet**: Select the subnet where the RDS for MySQL DB instance is, that is, the subnet selected in :ref:`Step 2: Create an RDS Database Table <dli_01_0020__section18912701608>`.

         To check the subnet information, go to the RDS console, choose **Instances** in the navigation pane on the left, click the name of the RDS for MySQL DB instance you created, and find **Subnet** in the **Connection Information** area on the displayed page.

   c. Click **OK**.
   d. In the **Enhanced** tab, click the created connection **dlirds** to view its **VPC Peering ID** and **Connection Status**. If the connection status is **Active**, the connection is successful.
   e. Test if the queue can connect to the RDS for MySQL DB instance.

      #. In the navigation pane on the left, choose **Resources** > **Queue Management**. On the displayed page, locate the queue added in :ref:`Step 3: Create an Elastic Resource Pool and Add Queues to the Pool <dli_01_0020__section1573781010172>`, click **More** in the **Operation** column, and select **Test Address Connectivity**.
      #. Enter the floating IP address of the RDS for MySQL DB instance recorded in :ref:`Step 2: Create an RDS Database Table <dli_01_0020__section18912701608>`.

         .. note::

            On the **Instance Management** page, click the target DB instance. On the displayed page, choose **Connection Information** > **Floating IP Address** to obtain the floating IP address.

         -  If the IP address is reachable, the DLI queue can connect to the RDS for MySQL DB instance through the created enhanced datasource connection.
         -  If the IP address is unreachable, check if the security group where the data source is allows access from the IP address of the elastic resource pool. Once the fault is rectified, retest the network connectivity.

.. _dli_01_0020__section1134817141019:

Step 5: Create a Datasource Authentication
------------------------------------------

When analyzing across multiple sources, you are advised not to configure authentication information directly in a job as it can lead to password leakage. Instead, you are advised to use datasource authentication provided by DLI to securely store data source authentication information.

To connect a Spark SQL job to an RDS for MySQL data source, you can create a password-type datasource authentication.

#. Log in to the DLI management console.

#. In the navigation pane on the left, choose **Datasource Connections**. On the displayed page, click **Datasource Authentication**.

#. Click **Create**.

   Set authentication parameters based on :ref:`Table 6 <dli_01_0020__table154674113817>`.

   .. _dli_01_0020__table154674113817:

   .. table:: **Table 6** Datasource authentication parameters

      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                      |
      +===================================+==================================================================================================================================+
      | Type                              | Select **Password**.                                                                                                             |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
      | Authentication Certificate        | Name of the datasource authentication to create                                                                                  |
      |                                   |                                                                                                                                  |
      |                                   | -  Only numbers, letters, and underscores (_) are allowed. The name cannot contain only numbers or start with an underscore (_). |
      |                                   | -  The value can contain a maximum of 128 characters.                                                                            |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
      | Username                          | Username for logging in to the RDS for MySQL DB instance                                                                         |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
      | Password                          | Password for logging in to the RDS for MySQL DB instance                                                                         |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------+

.. _dli_01_0020__section21590507141153:

Step 6: Submit a SQL Job
------------------------

In this example, a SQL job accesses an RDS table using a datasource connection.

#. On the DLI management console, choose **SQL Editor** in the navigation pane on the left.

#. In the editing window on the right of the **SQL Editor** page, enter the following SQL statement to create database **db1** and click **Execute**.

   .. code-block::

      create database db1;

#. On the top of the editing window, choose the **dli_queue_01** queue and the **db1** database. Enter the following SQL statements to create a table, insert data to the table, and query the data. Click **Execute**.

   View the query result to verify that the query is successful and the datasource connection works.

   .. code-block::

      CREATE TABLE IF NOT EXISTS rds_test USING JDBC OPTIONS (
      'url' = 'jdbc:mysql://{{ip}}:{{port}}',  // Private IP address and port of RDS
        'driver' = 'com.mysql.jdbc.Driver',
      'dbtable' = 'dli_demo.tabletest', // Name of the created DB instance and table name
          'passwdauth'="xxxxx" // Name of the datasource authentication of the password type created on DLI. If datasource authentication is used, you do not need to set the username and password for the job.
      )

      insert into rds_test VALUES ('123','abc');


      SELECT * from rds_test;
