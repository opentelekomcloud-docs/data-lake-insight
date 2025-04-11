:original_name: dli_08_15034.html

.. _dli_08_15034:

Source Table
============

Function
--------

Flink SQL jobs read from the Doris source table.

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

+----------------------------------+--------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Parameter                        | Default Value      | Mandatory | Parameter Type Description                                                                                                                                                                                                                                                                           |
+==================================+====================+===========+======================================================================================================================================================================================================================================================================================================+
| fenodes                          | --                 | Yes       | IP address and port number of the Doris FE. Use commas (,) to separate them for multiple instances. To obtain the port number, log in to MRS Manager, choose **Cluster** > **Services** > **Doris** > **Configurations**, and search for **http**. Search for **https** instead if HTTPS is enabled. |
+----------------------------------+--------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| table.identifier                 | --                 | Yes       | Doris table name, for example, **db.tbl**.                                                                                                                                                                                                                                                           |
+----------------------------------+--------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| username                         | --                 | Yes       | User name for accessing Doris.                                                                                                                                                                                                                                                                       |
+----------------------------------+--------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| password                         | --                 | Yes       | Password for accessing Doris.                                                                                                                                                                                                                                                                        |
+----------------------------------+--------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| doris.request.retries            | 3                  | No        | Number of retry times for sending requests to Doris.                                                                                                                                                                                                                                                 |
+----------------------------------+--------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| doris.request.connect.timeout.ms | 30000              | No        | Connection timeout interval for sending requests to Doris.                                                                                                                                                                                                                                           |
+----------------------------------+--------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| doris.request.read.timeout.ms    | 30000              | No        | Read timeout interval for sending requests to Doris.                                                                                                                                                                                                                                                 |
+----------------------------------+--------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| doris.request.query.timeout.s    | 3600               | No        | Timeout interval for querying Doris. The default value is **1** hour. The value **-1** indicates that there is no timeout limit.                                                                                                                                                                     |
+----------------------------------+--------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| doris.request.tablet.size        | Integer. MAX_VALUE | No        | Number of Doris Tablets corresponding to a partition. The smaller the value set, the more partitions will be generated. This increases the degree of parallelism in Flink, but puts more pressure on Doris.                                                                                          |
+----------------------------------+--------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| doris.batch.size                 | 1024               | No        | Maximum number of rows to read from BE at a time. Increasing this value reduces the number of times Flink needs to establish a connection with Doris when reading data from BE, thereby reducing the additional time overhead caused by network latency.                                             |
+----------------------------------+--------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| doris.exec.mem.limit             | 2147483648         | No        | Memory limit for a single query, with a default value of 2 GB, in bytes.                                                                                                                                                                                                                             |
+----------------------------------+--------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| doris.deserialize.arrow.async    | FALSE              | No        | Whether to support asynchronous conversion of Arrow format to RowBatch required for flink-doris-connector iteration.                                                                                                                                                                                 |
+----------------------------------+--------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| doris.deserialize.queue.size     | 64                 | No        | The internal processing queue for asynchronous conversion of Arrow format is effective when **doris.deserialize.arrow.async** is set to **true**.                                                                                                                                                    |
+----------------------------------+--------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| doris.read.field                 | --                 | No        | The column name list for reading from Doris tables, with multiple columns separated by commas.                                                                                                                                                                                                       |
+----------------------------------+--------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| doris.filter.query               | --                 | No        | The expression used to filter the data to be read, which is passed through to Doris. Doris uses this expression to filter the source data.                                                                                                                                                           |
+----------------------------------+--------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

This example reads data from a Doris source table and inputs it into the Print connector.

#. Create an enhanced datasource connection in the VPC and subnet where Doris locates, and bind the connection to the required Flink elastic resource pool. Add MRS host information for the enhanced datasource connection.

#. Set Doris security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the Doris address. If the connection passes the test, it is bound to the queue.

#. Create a Doris table and insert 10 data records into the table. The creation statement is as follows:

   .. code-block::

      CREATE TABLE IF NOT EXISTS dorisdemo
      (
        `user_id` varchar(10) NOT NULL,
        `city` varchar(10),
        `age` int,
        `gender` int
      )
      DISTRIBUTED BY HASH(`user_id`) BUCKETS 10;

      INSERT INTO dorisdemo VALUES ('user1', 'city1', 20, 1);
      INSERT INTO dorisdemo VALUES ('user2', 'city2', 21, 0);
      INSERT INTO dorisdemo VALUES ('user3', 'city3', 22, 1);
      INSERT INTO dorisdemo VALUES ('user4', 'city4', 23, 0);
      INSERT INTO dorisdemo VALUES ('user5', 'city5', 24, 1);
      INSERT INTO dorisdemo VALUES ('user6', 'city6', 25, 0);
      INSERT INTO dorisdemo VALUES ('user7', 'city7', 26, 1);
      INSERT INTO dorisdemo VALUES ('user8', 'city8', 27, 0);
      INSERT INTO dorisdemo VALUES ('user9', 'city9', 28, 1);
      INSERT INTO dorisdemo VALUES ('user10', 'city10', 29, 0);

#. Create a Flink OpenSource SQL job. Enter the following job script and submit the job. The job script reads from the Doris table and prints the output.

   .. code-block::

      CREATE TABLE dorisDemo (
        `user_id` String NOT NULL,
        `city` String,
        `age` int,
        `gender` int
      ) with (
        'connector' = 'doris',
        'fenodes' = 'FE_IP:PORT,FE_IP:PORT,FE_IP:PORT',
        'table.identifier' = 'demo.dorisdemo',
        'username' = 'dorisUser',
        'password' = 'dorisPassword',
        'doris.request.retries'='3',
        'doris.batch.size' = '100'
      );

      CREATE TABLE print (
        `user_id` String NOT NULL,
        `city` String,
        `age` int,
        `gender` int
      ) with (
        'connector' = 'print'
      );

      insert into print select * from dorisDemo;

#. View the data in the Print result table.

   .. code-block::

      +I[user5, city5, 24, 1]
      +I[user4, city4, 23, 0]
      +I[user3, city3, 22, 1]
      +I[user10, city10, 29, 0]
      +I[user6, city6, 25, 0]
      +I[user1, city1, 20, 1]
      +I[user9, city9, 28, 1]
      +I[user7, city7, 26, 1]
      +I[user8, city8, 27, 0]
      +I[user2, city2, 21, 0]
