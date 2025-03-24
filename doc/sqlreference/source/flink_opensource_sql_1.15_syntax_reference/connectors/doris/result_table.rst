:original_name: dli_08_15035.html

.. _dli_08_15035:

Result Table
============

Function
--------

Flink SQL jobs write to the Doris result table.

Prerequisites
-------------

-  An enhanced datasource connection has been created for DLI to connect to Doris, so that jobs can run on the dedicated queue of DLI and you can set the security group rules as required.

-  **If MRS Doris is used, IP addresses of all hosts in the MRS cluster have been added to host information of the enhanced datasource connection.**

   For details, see "Modifying Host Information" in *Data Lake Insight User Guide*.

-  Kerberos authentication is disabled for the cluster (the cluster is in normal mode)

   After connecting to Doris as user **admin**, create a role with administrator permissions, and bind the role to the user.

Caveats
-------

-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.
-  Storing authentication credentials such as usernames and passwords in code or plaintext poses significant security risks. It is recommended using DEW to manage credentials instead. Storing encrypted credentials in configuration files or environment variables and decrypting them when needed ensures security. For details, see .
-  Kerberos authentication is disabled for the cluster (the cluster is in normal mode)
-  Doris table names are case sensitive.
-  When Doris of CloudTable is used, set the port number in the **fenodes** field to **8030**, for example, *xx*\ **:8030**. In addition, enable ports **8030**, **8040**, and **9030** in the security group.
-  After HTTPS is enabled, add the following configuration parameters to the **with** clause for creating a table:

   -  **'doris.enable.https' = 'true'**
   -  **'doris.ignore.https.ca' = 'true'**

-  On the **Running Parameters** tab of the Flink job editing page, check **Enable Checkpointing**. Otherwise, data can be written to the Doris result table, and the delay in writing to Doris depends on the value set for **Checkpoint Interval**.

Syntax
------

.. code-block::

   create table dorisSource (
     attr_name attr_type
     (',' attr_name attr_type)*
    )
   with (
     'connector' = 'doris',
     'fenodes' = 'FE_IP:PORT,FE_IP:PORT,FE_IP:PORT',
     'table.identifier' = 'database.table',
     'username' = 'dorisUsername',
     'password' = 'dorisPassword'
   );

Parameter Description
---------------------

**Shared configuration**

+---------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Parameter           | Default Value   | Mandatory       | Parameter Type Description                                                                                                                                                                                                                                                                           |
+=====================+=================+=================+======================================================================================================================================================================================================================================================================================================+
| fenodes             | --              | Yes             | IP address and port number of the Doris FE. Use commas (,) to separate them for multiple instances. To obtain the port number, log in to MRS Manager, choose **Cluster** > **Services** > **Doris** > **Configurations**, and search for **http**. Search for **https** instead if HTTPS is enabled. |
+---------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| table.identifier    | --              | Yes             | Doris table name, for example, **db.tbl**.                                                                                                                                                                                                                                                           |
+---------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| username            | --              | Yes             | User name for accessing Doris.                                                                                                                                                                                                                                                                       |
+---------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| password            | --              | Yes             | Password for accessing Doris.                                                                                                                                                                                                                                                                        |
+---------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| sink.label-prefix   | ""              | Yes             | Label prefix used for Stream load import. It must be globally unique in two-phase commit (2pc) scenarios to ensure Flink's EOS semantics.                                                                                                                                                            |
+---------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| sink.enable-2pc     | TRUE            | No              | Whether to enable 2pc for ensuring Exactly-Once semantics. The default value is **true**.                                                                                                                                                                                                            |
+---------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| sink.check-interval | 10000           | No              | Interval for checking exceptions during loading.                                                                                                                                                                                                                                                     |
+---------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| sink.max-retries    | 3               | No              | Maximum number of retries when writing records to the database fails.                                                                                                                                                                                                                                |
+---------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| sink.buffer-size    | 256 \* 1024     | No              | Buffer size for caching data during Stream load.                                                                                                                                                                                                                                                     |
+---------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| sink.buffer-count   | 3               | No              | Buffer count for caching data during Stream load.                                                                                                                                                                                                                                                    |
+---------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| sink.enable-delete  | TRUE            | No              | Whether to enable deletion. This option requires batch deletion to be enabled for the Doris table (default in Doris 0.15 or later for Unique model only).                                                                                                                                            |
+---------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| sink.properties.\*  | --              | No              | Import parameters for Stream load.                                                                                                                                                                                                                                                                   |
|                     |                 |                 |                                                                                                                                                                                                                                                                                                      |
|                     |                 |                 | For example, **'sink.properties.column_separator' = ','** defines the column separator, and **'sink.properties.escape_delimiters' = 'true'** treats special characters as separators, where **'\\x01'** is converted to binary **0x01**.                                                             |
|                     |                 |                 |                                                                                                                                                                                                                                                                                                      |
|                     |                 |                 | JSON format import                                                                                                                                                                                                                                                                                   |
|                     |                 |                 |                                                                                                                                                                                                                                                                                                      |
|                     |                 |                 | 'sink.properties.format' = 'json' 'sink.properties.read_json_by_line' = 'true'                                                                                                                                                                                                                       |
+---------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

In this example, data is read from the DataGen data source and written to the Doris result table.

#. Create an enhanced datasource connection in the VPC and subnet where Doris locates, and bind the connection to the required Flink elastic resource pool. Add MRS host information for the enhanced datasource connection.

#. Set Doris security groups and add inbound rules to allow access from the Flink queue. Test the queue connectivity based on the Doris address. If the connection passes the test, it is bound to the queue.

#. Create a Doris table by referring to *MRS Doris Usage Guide*. The creation statement is as follows:

   .. code-block::

      CREATE TABLE IF NOT EXISTS dorisdemo
      (
        `user_id` varchar(10) NOT NULL,
        `city` varchar(10),
        `age` int,
        `gender` int
      )
      DISTRIBUTED BY HASH(`user_id`) BUCKETS 10

#. Create a Flink OpenSource SQL job. Enter the following job script and submit the job. The job script uses DataGen as the data source and writes data to as a Doris result table.

   .. code-block::

      create table student_datagen_source(
        `user_id` String NOT NULL,
        `city` String,
        `age` int,
        `gender` int
      ) with (
        'connector' = 'datagen',
        'rows-per-second' = '1',
        'fields.user_id.kind' = 'random',
        'fields.user_id.length' = '7',
        'fields.city.kind' = 'random',
        'fields.city.length' = '7'
      );


      CREATE TABLE dorisDemo (
        `user_id` String NOT NULL,
        `city` String,
        `age` int,
        `gender` int
      ) with (
        'connector' = 'doris',
        'fenodes' = 'FE_IP:PORT',
        'table.identifier' = 'demo.dorisdemo',
        'username' = 'dorisUser',
        'password' = 'dorisPassword',
        'sink.label-prefix' = 'demo',
        'sink.enable-2pc' = 'true',
        'sink.buffer-count' = '10'
      );

      insert into dorisDemo select * from student_datagen_source

5. Check whether data is successfully written to the Doris result table.

   ======= ======= === ======
   user_id city    age gender
   ======= ======= === ======
   50aff04 93406c5 12  1
   681a230 1f27d06 16  1
   006eff4 3521ded 18  0
   ======= ======= === ======
