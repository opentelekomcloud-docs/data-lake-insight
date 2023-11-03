:original_name: dli_08_0388.html

.. _dli_08_0388:

Postgres CDC Source Table
=========================

Function
--------

The Postgres CDC source table, that is, Postgres streaming source table, is used to read the full snapshot data and changed data of the PostgreSQL database in sequence. The exactly-once processing semantics is used to ensure data accuracy even if a failure occurs.

Prerequisites
-------------

-  The PostgreSQL version be 9.6, 10, 11, or 12.
-  An enhanced datasource connection with the database has been established, so that you can configure security group rules as required.

Precautions
-----------

-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.12** on the **Running Parameters** tab of the job editing page, select **Save Job Log**, and set the OBS bucket for saving job logs.

-  The PostgreSQL version cannot be earlier than PostgreSQL 11.

-  If operations such as update will be performed on the Postgres table, you need to run the following statement in PostgreSQL. Note: Replace **test.cdc_order** with the actual database and table.

   .. code-block::

      ALTER TABLE test.cdc_order REPLICA IDENTITY FULL

-  Before creating the PostgreSQL CDC source table, check whether the current PostgreSQL contains the default plug-in. You can run the following statement in PostgreSQL to query the current plug-ins:

   .. code-block::

      SELECT name FROM pg_available_extensions;

   If the default plug-in **decoderbufs** is not available, you need to set the **decoding.plugin.name** parameter to specify an existing plug-in in PostgreSQL when creating the PostgreSQL CDC source table.

Syntax
------

.. code-block::

   create table postgresCdcSource (
     attr_name attr_type
     (',' attr_name attr_type)*
     (','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
   )
   with (
     'connector' = 'postgres-cdc',
     'hostname' = 'PostgresHostname',
     'username' = 'PostgresUsername',
     'password' = 'PostgresPassword',
     'database-name' = 'PostgresDatabaseName',
     'schema-name' = 'PostgresSchemaName',
     'table-name' = 'PostgresTableName'
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +----------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter            | Mandatory   | Default Value | Data Type   | Description                                                                                                                                                                                                                                                                            |
   +======================+=============+===============+=============+========================================================================================================================================================================================================================================================================================+
   | connector            | Yes         | None          | String      | Connector to be used. Set this parameter to **postgres-cdc**.                                                                                                                                                                                                                          |
   +----------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | hostname             | Yes         | None          | String      | IP address or hostname of the Postgres database.                                                                                                                                                                                                                                       |
   +----------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | username             | Yes         | None          | String      | Username of the Postgres database.                                                                                                                                                                                                                                                     |
   +----------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password             | Yes         | None          | String      | Password of the Postgres database.                                                                                                                                                                                                                                                     |
   +----------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | database-name        | Yes         | None          | String      | Database name.                                                                                                                                                                                                                                                                         |
   +----------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | schema-name          | Yes         | None          | String      | Postgres schema name.                                                                                                                                                                                                                                                                  |
   |                      |             |               |             |                                                                                                                                                                                                                                                                                        |
   |                      |             |               |             | The schema name supports regular expressions to read data from multiple schemas. For example, **test(.)\*** indicates all schema names starting with **test**.                                                                                                                         |
   +----------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table-name           | Yes         | None          | String      | Postgres table name.                                                                                                                                                                                                                                                                   |
   |                      |             |               |             |                                                                                                                                                                                                                                                                                        |
   |                      |             |               |             | The table name supports regular expressions to read data from multiple tables. For example, **cdc_order(.)\*** indicates all table names starting with **cdc_order**.                                                                                                                  |
   +----------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | port                 | No          | 5432          | Integer     | Port number of the Postgres database.                                                                                                                                                                                                                                                  |
   +----------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | decoding.plugin.name | No          | decoderbufs   | String      | Determined based on the plug-in that is installed in the PostgreSQL database. The value can be:                                                                                                                                                                                        |
   |                      |             |               |             |                                                                                                                                                                                                                                                                                        |
   |                      |             |               |             | -  decoderbufs (default)                                                                                                                                                                                                                                                               |
   |                      |             |               |             | -  wal2json                                                                                                                                                                                                                                                                            |
   |                      |             |               |             | -  wal2json_rds                                                                                                                                                                                                                                                                        |
   |                      |             |               |             | -  wal2json_streaming                                                                                                                                                                                                                                                                  |
   |                      |             |               |             | -  wal2json_rds_streaming                                                                                                                                                                                                                                                              |
   |                      |             |               |             | -  pgoutput                                                                                                                                                                                                                                                                            |
   +----------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium.\*          | No          | None          | String      | Fine-grained control over the behavior of Debezium clients, for example, **'debezium.snapshot.mode' = 'never'**. For details, see `Connector configuration properties <https://debezium.io/documentation/reference/1.2/connectors/postgresql.html#postgresql-connector-properties>`__. |
   |                      |             |               |             |                                                                                                                                                                                                                                                                                        |
   |                      |             |               |             | You are advised to set the **debezium.slot.name** parameter for each table to avoid the following error: "PSQLException: ERROR: replication slot "debezium" is active for PID 974"                                                                                                     |
   +----------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

In this example, Postgres-CDC is used to read data from RDS for PostgreSQL in real time and write the data to the Print result table. The procedure is as follows (PostgreSQL 11.11 is used in this example):

#. Create an enhanced datasource connection in the VPC and subnet where PostgreSQL locates, and bind the connection to the required Flink elastic resource pool.

#. Set PostgreSQL security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the PostgreSQL address. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. In PostgreSQL, create database **flink** and schema **test**.

#. Create table **cdc_order** in the schema **test** of database **flink** in PostgreSQL.

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
        primary key(order_id)
      );

#. Run the following SQL statement in PostgreSQL. If you do not run this statement, an error will be reported when the Flink job is executed. For details, see the error message in :ref:`FAQ <dli_08_0388__en-us_topic_0000001310215785_li197238199359>`.

   .. code-block::

      ALTER TABLE test.cdc_order REPLICA IDENTITY FULL

#. Create a Flink OpenSource SQL job. Enter the following job script and submit the job.

   When you create a job, set **Flink Version** to **1.12** on the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

   .. code-block::

      create table postgresCdcSource(
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
        'hostname' = 'PostgresHostname',
        'username' = 'PostgresUsername',
        'password' = 'PostgresPassword',
        'database-name' = 'flink',
        'schema-name' = 'test',
        'table-name' = 'cdc_order'
      );

      create table printSink(
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
        'connector' = 'print'
      );

      insert into printSink select * from postgresCdcSource;

#. Run the following command in PostgreSQL:

   .. code-block::

      insert into test.cdc_order
        (order_id,
        order_channel,
        order_time,
        pay_amount,
        real_pay,
        pay_time,
        user_id,
        user_name,
        area_id) values
        ('202103241000000001', 'webShop', '2021-03-24 10:00:00', '100.00', '100.00', '2021-03-24 10:02:03', '0001', 'Alice', '330106'),
        ('202103251202020001', 'miniAppShop', '2021-03-25 12:02:02', '60.00', '60.00', '2021-03-25 12:03:00', '0002', 'Bob', '330110');

      update test.cdc_order set order_channel = 'webShop' where order_id = '202103251202020001';

      delete from test.cdc_order where order_id = '202103241000000001';

#. Perform the following operations to view the data result in the **taskmanager.out** file:

   a. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   b. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
   c. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **taskmanager.out** file, and view result logs.

   The data result is as follows:

   .. code-block::

      +I(202103241000000001,webShop,2021-03-24 10:00:00,100.0,100.0,2021-03-24 10:02:03,0001,Alice,330106)
      +I(202103251202020001,miniAppShop,2021-03-25 12:02:02,60.0,60.0,2021-03-25 12:03:00,0002,Bob,330110)
      -U(202103251202020001,miniAppShop,2021-03-25 12:02:02,60.0,60.0,2021-03-25 12:03:00,0002,Bob,330110)
      +U(202103251202020001,webShop,2021-03-25 12:02:02,60.0,60.0,2021-03-25 12:03:00,0002,Bob,330110)
      -D(202103241000000001,webShop,2021-03-24 10:00:00,100.0,100.0,2021-03-24 10:02:03,0001,Alice,330106)

FAQ
---

-  Q: What should I do if the Flink job execution fails and the log contains the following error information?

   .. code-block::

      org.postgresql.util.PSQLException: ERROR: logical decoding requires wal_level >= logical

-  A: Change the value of **wal_level** to **logical** and restart the PostgreSQL database.

   After modifying the PostgreSQL parameter, restart the RDS PostgreSQL instance for the modification to take effect.

-  .. _dli_08_0388__en-us_topic_0000001310215785_li197238199359:

   Q: What should I do if the Flink job execution fails and the log contains the following error information?

   .. code-block::

      java.lang.IllegalStateException: The "before" field of UPDATE/DELETE message is null, please check the Postgres table has been set REPLICA IDENTITY to FULL level. You can update the setting by running the command in Postgres 'ALTER TABLE test.cdc_order REPLICA IDENTITY FULL'.

   A: If a similar error is reported in the run log, run the **ALTER TABLE test.cdc_order REPLICA IDENTITY FULL** statement in PostgreSQL.
