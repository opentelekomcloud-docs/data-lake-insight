:original_name: dli_09_0013.html

.. _dli_09_0013:

Reading Data from PostgreSQL CDC and Writing Data to GaussDB(DWS)
=================================================================

.. important::

   This guide provides reference for Flink 1.12 only.

Description
-----------

Change Data Capture (CDC) can synchronize incremental changes from the source database to one or more destinations. During data synchronization, CDC processes data, for example, grouping (GROUP BY) and joining multiple tables (JOIN).

This example creates a PostgreSQL CDC source table to monitor PostgreSQL data changes and insert the changed data into a GaussDB(DWS) database.

Prerequisites
-------------

#. You have created an RDS for PostgreSQL DB instance. In this example, the RDS for PostgreSQL database version is 11.

   .. note::

      The version of the RDS PostgreSQL database cannot be earlier than 11.

#. You have created a GaussDB(DWS) instance.

Overall Process
---------------

:ref:`Figure 1 <dli_09_0013__en-us_topic_0000001269022188_fig1691441652>` shows the overall development process.

.. _dli_09_0013__en-us_topic_0000001269022188_fig1691441652:

.. figure:: /_static/images/en-us_image_0000001318542105.png
   :alt: **Figure 1** Job development process

   **Figure 1** Job development process

:ref:`Step 1: Create a Queue <dli_09_0013__en-us_topic_0000001269022188_section792923214216>`

:ref:`Step 2: Create an RDS PostgreSQL Database and Table <dli_09_0013__en-us_topic_0000001269022188_section1627154113018>`

:ref:`Step 3: Create a GaussDB(DWS) Database and Table <dli_09_0013__en-us_topic_0000001269022188_section1744572443816>`

:ref:`Step 4: Create an Enhanced Datasource Connection <dli_09_0013__en-us_topic_0000001269022188_section074025752119>`

:ref:`Step 5: Run a Job <dli_09_0013__en-us_topic_0000001269022188_section12448959174212>`

:ref:`Step 6: Send Data and Query Results <dli_09_0013__en-us_topic_0000001269022188_section4387527162418>`

.. _dli_09_0013__en-us_topic_0000001269022188_section792923214216:

Step 1: Create a Queue
----------------------

#. Log in to the DLI console. In the navigation pane on the left, choose **Resources** > **Queue Management**.
#. On the displayed page, click **Buy Queue** in the upper right corner.
#. On the **Buy Queue** page, set queue parameters as follows:

   -  **Billing Mode**: .
   -  **Region** and **Project**: Retain the default values.
   -  **Name**: Enter a queue name.

      .. note::

         The queue name can contain only digits, letters, and underscores (_), but cannot contain only digits or start with an underscore (_). The name must contain 1 to 128 characters.

         **The queue name is case-insensitive. Uppercase letters will be automatically converted to lowercase letters.**

   -  **Type**: Select **For general purpose**. Select the **Dedicated Resource Mode**.
   -  **AZ Mode** and **Specifications**: Retain the default values.
   -  **Enterprise Project**: Select **default**.
   -  **Advanced Settings**: Select **Custom**.
   -  **CIDR Block**: Specify the queue network segment. For example, **10.0.0.0/16**.

      .. caution::

         The CIDR block of a queue cannot overlap with the CIDR blocks of DMS Kafka and RDS for MySQL DB instances. Otherwise, datasource connections will fail to be created.

   -  Set other parameters as required.

#. Click **Buy**. Confirm the configuration and click **Submit**.

.. _dli_09_0013__en-us_topic_0000001269022188_section1627154113018:

Step 2: Create an RDS PostgreSQL Database and Table
---------------------------------------------------

#. Log in to the RDS console. On the displayed page, locate the target PostgreSQL DB instance and choose **More** > **Log In** in the **Operation** column.

#. In the login dialog box displayed, enter the username and password and click **Log In**.

#. Create a database instance and name it **testrdsdb**.

#. Create a schema named **test** for the **testrdsdb** database.

#. Choose **SQL Operations** > **SQL Query**. On the page displayed, create a RDS for PostgreSQL table.

   .. code-block::

      create table test.cdc_order(
        order_id VARCHAR,
        order_channel VARCHAR,
        order_time VARCHAR,
        pay_amount FLOAT8,
        real_pay FLOAT8,
        pay_time VARCHAR,
        user_id VARCHAR,
        user_name VARCHAR,
        area_id VARCHAR,
        primary key(order_id));

   Run the following statement in the PostgreSQL instance:

   .. code-block::

      ALTER TABLE test.cdc_order REPLICA IDENTITY FULL;

.. _dli_09_0013__en-us_topic_0000001269022188_section1744572443816:

Step 3: Create a GaussDB(DWS) Database and Table
------------------------------------------------

#. Connect to the created GaussDB(DWS) cluster.

#. Connect to the default database **gaussdb** of a GaussDB(DWS) cluster.

   .. code-block::

      gsql -d gaussdb -h Connection address of the GaussDB(DWS) cluster -U dbadmin -p 8000 -W password -r

   -  **gaussdb**: Default database of the GaussDB(DWS) cluster
   -  **Connection address of the DWS cluster**: If a public network address is used for connection, set this parameter to the public network IP address or domain name. If a private network address is used for connection, set this parameter to the private network IP address or domain name. If an ELB is used for connection, set this parameter to the ELB address.
   -  **dbadmin**: Default administrator username used during cluster creation
   -  **-W**: Default password of the administrator

#. Run the following command to create the **testdwsdb** database:

   .. code-block::

      CREATE DATABASE testdwsdb;

#. Run the following command to exit the **gaussdb** database and connect to **testdwsdb**:

   .. code-block::

      \q
      gsql -d testdwsdb -h Connection address of the GaussDB(DWS) cluster -U dbadmin -p 8000 -W password -r

#. Run the following commands to create a table:

   .. code-block::

      create schema test;
      set current_schema= test;
      drop table if exists dws_order;
      CREATE TABLE dws_order
      (
        order_id VARCHAR,
        order_channel VARCHAR,
        order_time VARCHAR,
        pay_amount FLOAT8,
        real_pay FLOAT8,
        pay_time VARCHAR,
        user_id VARCHAR,
        user_name VARCHAR,
        area_id VARCHAR
      );

.. _dli_09_0013__en-us_topic_0000001269022188_section074025752119:

Step 4: Create an Enhanced Datasource Connection
------------------------------------------------

-  **Connecting DLI to RDS**

   #. Go to the RDS console, click the name of the target RDS DB instance on the **Instances** page. Basic information of the instance is displayed.

   #. .. _dli_09_0013__en-us_topic_0000001269022188_li1976917464396:

      In the **Connection Information** pane, obtain the floating IP address, database port, VPC, and subnet.

   #. Click the security group name. On the displayed page, click the **Inbound Rules** tab and add a rule to allow access from DLI queues. For example, if the CIDR block of the queue is 10.0.0.0/16, set **Priority** to **1**, **Action** to **Allow**, **Protocol** to **TCP**, **Type** to **IPv4**, **Source** to **10.0.0.0/16**, and click **OK**.

   #. Log in to the DLI management console. In the navigation pane on the left, choose **Datasource Connections**. On the displayed page, click **Create** in the **Enhanced** tab.

   #. In the displayed dialog box, set the following parameters: For details, see the following section:

      -  **Connection Name**: Enter a name for the enhanced datasource connection. For this example, enter **dli_rds**.
      -  **Resource Pool**: Select the name of the queue created in :ref:`Step 1: Create a Queue <dli_09_0013__en-us_topic_0000001269022188_section792923214216>`.
      -  **VPC**: Select the VPC of the RDS DB instance.
      -  **Subnet**: Select the subnet of RDS DB instance.
      -  Set other parameters as you need.

      Click **OK**. Click the name of the created datasource connection to view its status. You can perform subsequent steps only after the connection status changes to **Active**.

   #. In the navigation pane on the left, choose **Resources** > **Queue Management**. On the page displayed, locate the queue you created in :ref:`Step 1: Create a Queue <dli_09_0013__en-us_topic_0000001269022188_section792923214216>`, click **More** in the **Operation** column, and select **Test Address Connectivity**.

   #. In the displayed dialog box, enter *floating IP address*\ **:**\ *database port* of the RDS DB instance you have obtained in :ref:`2 <dli_09_0013__en-us_topic_0000001269022188_li1976917464396>` in the **Address** box and click **Test** to check whether the database is reachable.

-  **Connecting DLI to GaussDB(DWS)**

   #. On the GaussDB(DWS) management console, choose **Clusters**. On the displayed page, click the name of the created GaussDB(DWS) cluster to view basic information.

   #. .. _dli_09_0013__en-us_topic_0000001269022188_li19666016361:

      In the Basic Information tab, locate the **Database Attributes** pane and obtain the private IP address and port number of the DB instance. In the **Network** pane, obtain VPC, and subnet information.

   #. Click the security group name. On the displayed page, click the **Inbound Rules** tab and add a rule to allow access from DLI queues. For example, if the CIDR block of the queue is 10.0.0.0/16, set **Priority** to **1**, **Action** to **Allow**, **Protocol** to **TCP**, **Type** to **IPv4**, **Source** to **10.0.0.0/16**, and click **OK**.

   #. Check whether the RDS instance and GaussDB(DWS) instance are in the same VPC and subnet.

      a. If they are, go to :ref:`7 <dli_09_0013__en-us_topic_0000001269022188_li9816175412318>`. You do not need to create an enhanced datasource connection again.
      b. If they are not, go to :ref:`5 <dli_09_0013__en-us_topic_0000001269022188_li11976319011>`. Create an enhanced datasource connection to connect RDS to the subnet where the GaussDB(DWS) instance locates.

   #. .. _dli_09_0013__en-us_topic_0000001269022188_li11976319011:

      Log in to the DLI management console. In the navigation pane on the left, choose **Datasource Connections**. On the displayed page, click **Create** in the **Enhanced** tab.

   #. In the displayed dialog box, set the following parameters: For details, see the following section:

      -  **Connection Name**: Enter a name for the enhanced datasource connection. For this example, enter **dli_dws**.
      -  **Resource Pool**: Select the name of the queue created in :ref:`Step 1: Create a Queue <dli_09_0013__en-us_topic_0000001269022188_section792923214216>`.
      -  **VPC**: Select the VPC of the GaussDB(DWS) instance.
      -  **Subnet**: Select the subnet of GaussDB(DWS) instance.
      -  Set other parameters as you need.

      Click **OK**. Click the name of the created datasource connection to view its status. You can perform subsequent steps only after the connection status changes to **Active**.

   #. .. _dli_09_0013__en-us_topic_0000001269022188_li9816175412318:

      In the navigation pane on the left, choose **Resources** > **Queue Management**. On the page displayed, locate the queue you created in :ref:`Step 1: Create a Queue <dli_09_0013__en-us_topic_0000001269022188_section792923214216>`, click **More** in the **Operation** column, and select **Test Address Connectivity**.

   #. In the displayed dialog box, enter *floating IP address*\ **:**\ *database port* of the GaussDB(DWS) instance you have obtained in :ref:`2 <dli_09_0013__en-us_topic_0000001269022188_li19666016361>` in the **Address** box and click **Test** to check whether the database is reachable.

.. _dli_09_0013__en-us_topic_0000001269022188_section12448959174212:

Step 5: Run a Job
-----------------

#. On the DLI management console, choose **Job Management** > **Flink Jobs**. On the **Flink Jobs** page, click **Create Job**.
#. In the **Create Job** dialog box, set **Type** to **Flink OpenSource SQL** and **Name** to **FlinkCDCPostgreDWS**. Click **OK**.
#. On the job editing page, set the following parameters and retain the default values of other parameters.

   -  **Queue**: Select the queue created in :ref:`Step 1: Create a Queue <dli_09_0013__en-us_topic_0000001269022188_section792923214216>`.

   -  **Flink Version**: Select **1.12**.

   -  **Save Job Log**: Enable this function.

   -  **OBS Bucket**: Select an OBS bucket for storing job logs and grant access permissions of the OBS bucket as prompted.

   -  **Enable Checkpointing**: Enable this function.

   -  Enter a SQL statement in the editing pane. The following is an example. Modify the parameters in bold as you need.

      .. note::

         In this example, the syntax version of Flink OpenSource SQL is 1.12. In this example, the data source is Kafka and the result data is written to Elasticsearch.

      .. table:: **Table 1** Job running parameters

         +-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                           | Description                                                                                                                                                                                                                                                         |
         +=====================================+=====================================================================================================================================================================================================================================================================+
         | Queue                               | A shared queue is selected by default. You can select a CCE queue with dedicated resources and configure the following parameters:                                                                                                                                  |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | **UDF Jar**: UDF Jar file. Before selecting such a file, upload the corresponding JAR file to the OBS bucket and choose **Data Management** > **Package Management** to create a package. For details, see .                                                        |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | In SQL, you can call a UDF that is inserted into a JAR file.                                                                                                                                                                                                        |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | .. note::                                                                                                                                                                                                                                                           |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     |    When creating a job, a sub-user can only select the queue that has been allocated to the user.                                                                                                                                                                   |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     |    If the remaining capacity of the selected queue cannot meet the job requirements, the system automatically scales up the capacity and you will be billed based on the increased capacity. When a queue is idle, the system automatically scales in its capacity. |
         +-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | CUs                                 | Sum of the number of compute units and job manager CUs of DLI. One CU equals 1 vCPU and 4 GB.                                                                                                                                                                       |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | The value is the number of CUs required for job running and cannot exceed the number of CUs in the bound queue.                                                                                                                                                     |
         +-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Job Manager CUs                     | Number of CUs of the management unit.                                                                                                                                                                                                                               |
         +-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parallelism                         | Maximum number of Flink OpenSource SQL jobs that can run at the same time.                                                                                                                                                                                          |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | .. note::                                                                                                                                                                                                                                                           |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     |    This value cannot be greater than four times the compute units (number of CUs minus the number of JobManager CUs).                                                                                                                                               |
         +-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Task Manager Configuration          | Whether to set Task Manager resource parameters.                                                                                                                                                                                                                    |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | If this option is selected, you need to set the following parameters:                                                                                                                                                                                               |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | -  **CU(s) per TM**: Number of resources occupied by each Task Manager.                                                                                                                                                                                             |
         |                                     | -  **Slot(s) per TM**: Number of slots contained in each Task Manager.                                                                                                                                                                                              |
         +-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | OBS Bucket                          | OBS bucket to store job logs and checkpoint information. If the selected OBS bucket is not authorized, click **Authorize**.                                                                                                                                         |
         +-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Save Job Log                        | Whether to save job run logs to OBS. The logs are saved in *Bucket name*\ **/jobs/logs/**\ *Directory starting with the job ID*.                                                                                                                                    |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | .. caution::                                                                                                                                                                                                                                                        |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     |    CAUTION:                                                                                                                                                                                                                                                         |
         |                                     |    You are advised to configure this parameter. Otherwise, no run log is generated after the job is executed. If the job fails, the run log cannot be obtained for fault locating.                                                                                  |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | If this option is selected, you need to set the following parameters:                                                                                                                                                                                               |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | **OBS Bucket**: Select an OBS bucket to store user job logs. If the selected OBS bucket is not authorized, click **Authorize**.                                                                                                                                     |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | .. note::                                                                                                                                                                                                                                                           |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     |    If **Enable Checkpointing** and **Save Job Log** are both selected, you only need to authorize OBS once.                                                                                                                                                         |
         +-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Alarm Generation upon Job Exception | Whether to notify users of any job exceptions, such as running exceptions or arrears, via SMS or email.                                                                                                                                                             |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | If this option is selected, you need to set the following parameters:                                                                                                                                                                                               |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | **SMN Topic**                                                                                                                                                                                                                                                       |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | Select a user-defined SMN topic. For details about how to create a custom SMN topic, see "Creating a Topic" in *Simple Message Notification User Guide*.                                                                                                            |
         +-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Enable Checkpointing                | Whether to enable job snapshots. If this function is enabled, jobs can be restored based on checkpoints.                                                                                                                                                            |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | If this option is selected, you need to set the following parameters:                                                                                                                                                                                               |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | -  **Checkpoint Interval**: interval for creating checkpoints, in seconds. The value ranges from 1 to 999999, and the default value is **30**.                                                                                                                      |
         |                                     | -  **Checkpoint Mode**: checkpointing mode, which can be set to either of the following values:                                                                                                                                                                     |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     |    -  **At least once**: Events are processed at least once.                                                                                                                                                                                                        |
         |                                     |    -  **Exactly once**: Events are processed only once.                                                                                                                                                                                                             |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | -  **OBS Bucket**: Select an OBS bucket to store your checkpoints. If the selected OBS bucket is not authorized, click **Authorize**.                                                                                                                               |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     |    Checkpoints are saved in *Bucket name*\ **/jobs/checkpoint/**\ *Directory starting with the job ID*.                                                                                                                                                             |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     |    .. note::                                                                                                                                                                                                                                                        |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     |       If **Enable Checkpointing** and **Save Job Log** are both selected, you only need to authorize OBS once.                                                                                                                                                      |
         +-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Auto Restart upon Exception         | Whether to enable automatic restart. If this function is enabled, jobs will be automatically restarted and restored when exceptions occur.                                                                                                                          |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | If this option is selected, you need to set the following parameters:                                                                                                                                                                                               |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | -  **Max. Retry Attempts**: maximum number of retries upon an exception. The unit is times/hour.                                                                                                                                                                    |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     |    -  **Unlimited**: The number of retries is unlimited.                                                                                                                                                                                                            |
         |                                     |    -  **Limited**: The number of retries is user-defined.                                                                                                                                                                                                           |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | -  **Restore Job from Checkpoint**: This parameter is available only when **Enable Checkpointing** is selected.                                                                                                                                                     |
         +-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Idle State Retention Time           | How long the state of a key is retained without being updated before it is removed in **GroupBy** or **Window**. The default value is 1 hour.                                                                                                                       |
         +-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Dirty Data Policy                   | Policy for processing dirty data. The following policies are supported: **Ignore**, **Trigger a job exception**, and **Save**.                                                                                                                                      |
         |                                     |                                                                                                                                                                                                                                                                     |
         |                                     | If you set this field to **Save**, **Dirty Data Dump Address** must be set. Click the address box to select the OBS path for storing dirty data.                                                                                                                    |
         +-------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

      .. code-block::

         create table PostgreCdcSource(
           order_id string,
           order_channel string,
           order_time string,
           pay_amount double,
           real_pay double,
           pay_time string,
           user_id string,
           user_name string,
           area_id STRING,
           primary key (order_id) not enforced
         ) with (
           'connector' = 'postgres-cdc',
           'hostname' = ' 192.168.15.153',--IP address of the PostgreSQL instance
           'port'= ' 5432',--Port number of the PostgreSQL instance
           'pwd_auth_name'= 'xxxxx', -- Name of the datasource authentication of the password type created on DLI. If datasource authentication is used, you do not need to set the username and password for the job.
           'database-name' = ' testrdsdb',--Database name of the PostgreSQL instance
           'schema-name' = ' test',-- Schema in the PostgreSQL database
           'table-name' = ' cdc_order'--Table name in the PostgreSQL database
         );

         create table dwsSink(
           order_id string,
           order_channel string,
           order_time string,
           pay_amount double,
           real_pay double,
           pay_time string,
           user_id string,
           user_name string,
           area_id STRING,
           primary key(order_id) not enforced
         ) with (
           'connector' = 'gaussdb',
           'driver' = 'com.gauss200.jdbc.Driver',
           'url'='jdbc:gaussdb://192.168.168.16:8000/testdwsdb ', ---192.168.168.16:8000 indicates the internal IP address and port of the GaussDB(DWS) instance. testdwsdb indicates the name of the created GaussDB(DWS) database.
           'table-name' = ' test\".\"dws_order', ---test indicates the schema of the created GaussDB(DWS) table, and dws_order indicates the GaussDB(DWS) table name.
           'username' = 'xxxxx',--Username of the GaussDB(DWS) instance
           'password' = 'xxxxx',--Password of the GaussDB(DWS) instance
           'write.mode' = 'insert'
         );

         insert into dwsSink select * from PostgreCdcSource where pay_amount > 100;

#. Click **Check Semantic** and ensure that the SQL statement passes the check. Click **Save**. Click **Start**, confirm the job parameters, and click **Start Now** to execute the job. Wait until the job status changes to **Running**.

.. _dli_09_0013__en-us_topic_0000001269022188_section4387527162418:

Step 6: Send Data and Query Results
-----------------------------------

#. Log in to the RDS console. On the displayed page, locate the target PostgreSQL DB instance and choose **More** > **Log In** in the **Operation** column.

#. On the displayed login dialog box, enter the username and password and click **Log In**.

#. In the **Operation** column of row where the created database locates, click **SQL Window** and enter the following statement to create a table and insert data to the table:

   .. code-block::

      insert into test.cdc_order values
      ('202103241000000001','webShop','2021-03-24 10:00:00','50.00','100.00','2021-03-24 10:02:03','0001','Alice','330106'),
      ('202103251606060001','appShop','2021-03-24 12:06:06','200.00','180.00','2021-03-24 16:10:06','0002','Jason','330106'),
      ('202103261000000001','webShop','2021-03-24 14:03:00','300.00','100.00','2021-03-24 10:02:03','0003','Lily','330106'),
      ('202103271606060001','appShop','2021-03-24 16:36:06','99.00','150.00','2021-03-24 16:10:06','0001','Henry','330106');

#. Connect to the created GaussDB(DWS) cluster.

#. Connect to the default database **testdwsdb** of a GaussDB(DWS) cluster.

   .. code-block::

      gsql -d testdwsdb -h Connection address of the GaussDB(DWS) cluster -U dbadmin -p 8000 -W password -r

#. Run the following statements to query table data:

   .. code-block::

      select * from test.dws_order;

   The query result is as follows:

   .. code-block::

      order_channel              order_channel     order_time             pay_amount  real_pay  pay_time              user_id  user_name  area_id
      202103251606060001         appShop         2021-03-24 12:06:06       200.0      180.0   2021-03-24 16:10:06      0002      Jason     330106
      202103261000000001         webShop         2021-03-24 14:03:00       300.0      100.0   2021-03-24 10:02:03      0003      Lily      330106
