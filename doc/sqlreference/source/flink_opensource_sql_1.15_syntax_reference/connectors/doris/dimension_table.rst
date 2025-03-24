:original_name: dli_08_15036.html

.. _dli_08_15036:

Dimension Table
===============

Function
--------

Create a Doris dimension table to connect to the source streams for wide table generation.

Prerequisites
-------------

-  An enhanced datasource connection has been created for DLI to connect to HBase, so that jobs can run on the dedicated queue of DLI and you can set the security group rules as required.

-  **If MRS Doris is used, IP addresses of all hosts in the MRS cluster have been added to host information of the enhanced datasource connection.**

   For details, see "Modifying Host Information" in *Data Lake Insight User Guide*.

-  Kerberos authentication is disabled for the cluster (the cluster is in normal mode).

   After connecting to Doris as user **admin**, create a role with administrator permissions, and bind the role to the user.

Caveats
-------

-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.
-  Storing authentication credentials such as usernames and passwords in code or plaintext poses significant security risks. It is recommended using DEW to manage credentials instead. Storing encrypted credentials in configuration files or environment variables and decrypting them when needed ensures security. For details, see .
-  Kerberos authentication is disabled for the cluster (the cluster is in normal mode).
-  Doris table names are case sensitive.
-  When Doris of CloudTable is used, set the port number in the **fenodes** field to **8030**, for example, *xx*\ **:8030**. In addition, enable ports **8030**, **8040**, and **9030** in the security group.
-  After HTTPS is enabled, add the following configuration parameters to the **with** clause for creating a table:

   -  **'doris.enable.https' = 'true'**
   -  **'doris.ignore.https.ca' = 'true'**

Syntax
------

.. code-block::

   create table hbaseSource (
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

+-----------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Parameter             | Default Value   | Mandatory       | Parameter Type Description                                                                                                                                                                                                                                                                           |
+=======================+=================+=================+======================================================================================================================================================================================================================================================================================================+
| fenodes               | --              | Y               | IP address and port number of the Doris FE. Use commas (,) to separate them for multiple instances. To obtain the port number, log in to MRS Manager, choose **Cluster** > **Services** > **Doris** > **Configurations**, and search for **http**. Search for **https** instead if HTTPS is enabled. |
+-----------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| table.identifier      | --              | Y               | Doris table name, for example, **db.tbl**.                                                                                                                                                                                                                                                           |
+-----------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| username              | --              | Y               | User name for accessing Doris.                                                                                                                                                                                                                                                                       |
+-----------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| password              | --              | Y               | Password for accessing Doris.                                                                                                                                                                                                                                                                        |
+-----------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| lookup.cache.max-rows | -1L             | N               | Maximum number of rows to search in the cache, where the oldest row will be deleted if this value is exceeded.                                                                                                                                                                                       |
|                       |                 |                 |                                                                                                                                                                                                                                                                                                      |
|                       |                 |                 | To enable cache configuration, both the **cache.max-rows** and **cache.ttl** options must be specified.                                                                                                                                                                                              |
+-----------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| lookup.cache.ttl      | 10s             | N               | Cache lifespan.                                                                                                                                                                                                                                                                                      |
+-----------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| lookup.max-retries    | 3               | N               | Maximum number of retry attempts when a database lookup fails.                                                                                                                                                                                                                                       |
+-----------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

This example reads data from a Doris source table and inputs it into the Print connector.

#. Create an enhanced datasource connection in the VPC and subnet where Doris locates, and bind the connection to the required Flink elastic resource pool. Add MRS host information for the enhanced datasource connection.

#. Set Doris and Kafka security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the Doris and Kafka addresses. If the connection passes the test, it is bound to the queue.

#. Create a Doris table and insert 10 data records. The creation statement is as follows:

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

#. Create a Flink OpenSource SQL job. Enter the following job script and submit the job. This job simulates reading data from Kafka, performs a join with a Doris dimension table to denormalize the data, and outputs it to Print.

   .. code-block::

      CREATE TABLE ordersSource (
        user_id string,
        user_name string,
        proctime as Proctime()
      ) WITH (
        'connector' = 'kafka',
        'topic' = 'kafka-topic',
        'properties.bootstrap.servers' = 'kafkaIp:port,kafkaIp:port,kafkaIp:port',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'json'
      );

      CREATE TABLE dorisDemo (
        `user_id` String NOT NULL,
        `city` String,
        `age` int,
        `gender` int
      ) with (
        'connector' = 'doris',
        'fenodes' = 'IP address of the FE instance:Port number',
        'table.identifier' = 'demo.dorisdemo',
        'username' = 'dorisUsername',
        'password' = 'dorisPassword',
        'lookup.cache.ttl'='10 m',
        'lookup.cache.max-rows' = '100'
      );

      CREATE TABLE print (
        user_id string,
        user_name string,
        `city` String,
        `age` int,
        `gender` int
      ) WITH (
        'connector' = 'print'
      );

      insert into print
      select
        orders.user_id,
        orders.user_name,
        dim.city,
        dim.age,
        dim.sex
      from ordersSource orders
      left join dorisDemo for system_time as of orders.proctime as dim on orders.user_id = dim.user_id;

#. Write two data records to the Kafka data source.

   .. code-block::

      {"user_id": "user1", "user_name": "name1"}
      {"user_id": "user2", "user_name": "name2"}

#. View the data in the Print result table.

   .. code-block::

      +I[user1, name1, city1, 20, 1]
      +I[user2, name2, city2, 21, 0]
