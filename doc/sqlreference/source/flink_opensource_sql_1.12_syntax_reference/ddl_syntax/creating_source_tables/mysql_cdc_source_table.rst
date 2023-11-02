:original_name: dli_08_0387.html

.. _dli_08_0387:

MySQL CDC Source Table
======================

Function
--------

The MySQL CDC source table, that is, the MySQL streaming source table, reads all historical data in the database first and then smoothly switches data read to the Binlog to ensure data integrity.

Prerequisites
-------------

-  MySQL CDC requires MySQL 5.7 or 8.0.\ *x*.
-  An enhanced datasource connection has been created for DLI to connect to the MySQL database, so that you can configure security group rules as required.
-  Binlog is enabled for MySQL, and **binlog_row_image** is set to **FULL**.
-  A MySQL user has been created and granted the **SELECT**, **SHOW DATABASES**, **REPLICATION SLAVE**, and **REPLICATION CLIENT** permissions.

Precautions
-----------

-  When creating a Flink OpenSource SQL job, you need to set **Flink Version** to **1.12** on the **Running Parameters** tab of the job editing page, select **Save Job Log**, and set the OBS bucket for saving job logs.

-  Each client that synchronizes database data has a unique ID, that is, the server ID. You are advised to configure a unique server ID for each MySQL CDC job in the same database.

   Main reasons are as follows:

   -  The MySQL server maintains the network connection and Binlog location based on the ID. Therefore, if a large number of clients with the same server ID connect to the MySQL server, the CPU usage of the MySQL server may increase sharply, affecting the stability of online services.
   -  If multiple jobs share the same server ID, Binlog locations will be disordered, making data read inaccurate. Therefore, you are advised to configure different server IDs for each MySQL CDC job.

-  Watermarks cannot be defined for MySQL CDC source tables. For details about window aggregation, see :ref:`FAQ <dli_08_0387__en-us_topic_0000001262815682_section09412772314>`.
-  If you connect to a sink source that supports upsert, such as GaussDB(DWS) and MySQL, you need to define the primary key in the statement for creating the sink table. For details, see the printSink table creation statement in :ref:`Example <dli_08_0387__en-us_topic_0000001262815682_section2787228135313>`.

Syntax
------

.. code-block::

   create table mySqlCdcSource (
     attr_name attr_type
     (',' attr_name attr_type)*
     (','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
   )
   with (
     'connector' = 'mysql-cdc',
     'hostname' = 'mysqlHostname',
     'username' = 'mysqlUsername',
     'password' = 'mysqlPassword',
     'database-name' = 'mysqlDatabaseName',
     'table-name' = 'mysqlTableName'
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +-------------------+-------------+----------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter         | Mandatory   | Default Value                    | Data Type   | Description                                                                                                                                                                                                                                    |
   +===================+=============+==================================+=============+================================================================================================================================================================================================================================================+
   | connector         | Yes         | None                             | String      | Connector to be used. Set this parameter to **mysql-cdc**.                                                                                                                                                                                     |
   +-------------------+-------------+----------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | hostname          | Yes         | None                             | String      | IP address or hostname of the MySQL database.                                                                                                                                                                                                  |
   +-------------------+-------------+----------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | username          | Yes         | None                             | String      | Username of the MySQL database.                                                                                                                                                                                                                |
   +-------------------+-------------+----------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password          | Yes         | None                             | String      | Password of the MySQL database.                                                                                                                                                                                                                |
   +-------------------+-------------+----------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | database-name     | Yes         | None                             | String      | Name of the database to connect.                                                                                                                                                                                                               |
   |                   |             |                                  |             |                                                                                                                                                                                                                                                |
   |                   |             |                                  |             | The database name supports regular expressions to read data from multiple databases. For example, **flink(.)\*** indicates all database names starting with **flink**.                                                                         |
   +-------------------+-------------+----------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table-name        | Yes         | None                             | String      | Name of the table to read data from.                                                                                                                                                                                                           |
   |                   |             |                                  |             |                                                                                                                                                                                                                                                |
   |                   |             |                                  |             | The table name supports regular expressions to read data from multiple tables. For example, **cdc_order(.)\*** indicates all table names starting with **cdc_order**.                                                                          |
   +-------------------+-------------+----------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | port              | No          | 3306                             | Integer     | Port number of the MySQL database.                                                                                                                                                                                                             |
   +-------------------+-------------+----------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | server-id         | No          | A random value from 5400 to 6000 | String      | A numeric ID of the database client, which must be globally unique in the MySQL cluster. You are advised to set a unique ID for each job in the same database.                                                                                 |
   |                   |             |                                  |             |                                                                                                                                                                                                                                                |
   |                   |             |                                  |             | By default, a random value ranging from 5400 to 6400 is generated.                                                                                                                                                                             |
   +-------------------+-------------+----------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.startup.mode | No          | initial                          | String      | Startup mode for consuming data.                                                                                                                                                                                                               |
   |                   |             |                                  |             |                                                                                                                                                                                                                                                |
   |                   |             |                                  |             | -  **initial** (default): In the first startup, the database scans all historical data and then reads the latest Binlog data.                                                                                                                  |
   |                   |             |                                  |             | -  **latest-offset**: In the first startup, the database reads data directly from the end of the Binlog (the latest Binlog) instead of scanning all historical data. That is, it reads only the latest changes after the connector is started. |
   +-------------------+-------------+----------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | server-time-zone  | No          | None                             | String      | Time zone of the session used by the database.                                                                                                                                                                                                 |
   +-------------------+-------------+----------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_08_0387__en-us_topic_0000001262815682_section2787228135313:

Example
-------

In this example, MySQL-CDC is used to read data from RDS for MySQL in real time and write the data to the Print result table. The procedure is as follows (MySQL 5.7.32 is used in this example):

#. Create an enhanced datasource connection in the VPC and subnet where MySQL locates, and bind the connection to the required Flink elastic resource pool.

#. Set MySQL security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the MySQL address. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Create a table named **cdc_order** in database **flink** of the MySQL database.

   .. code-block::

      CREATE TABLE `flink`.`cdc_order` (
          `order_id` VARCHAR(32) NOT NULL,
          `order_channel` VARCHAR(32) NULL,
          `order_time` VARCHAR(32) NULL,
          `pay_amount` DOUBLE  NULL,
          `real_pay` DOUBLE  NULL,
          `pay_time` VARCHAR(32) NULL,
          `user_id` VARCHAR(32) NULL,
          `user_name` VARCHAR(32) NULL,
          `area_id` VARCHAR(32) NULL,
          PRIMARY KEY (`order_id`)
      )   ENGINE = InnoDB
          DEFAULT CHARACTER SET = utf8mb4
          COLLATE = utf8mb4_general_ci;

#. Create a Flink OpenSource SQL job. Enter the following job script and submit the job.

   When you create a job, set **Flink Version** to **1.12** on the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

   .. code-block::

      create table mysqlCdcSource(
        order_id string,
        order_channel string,
        order_time string,
        pay_amount double,
        real_pay double,
        pay_time string,
        user_id string,
        user_name string,
        area_id STRING
      ) with (
        'connector' = 'mysql-cdc',
        'hostname' = 'mysqlHostname',
        'username' = 'mysqlUsername',
        'password' = 'mysqlPassword',
        'database-name' = 'mysqlDatabaseName',
        'table-name' = 'mysqlTableName'
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

      insert into printSink select * from mysqlCdcSource;

#. Insert test data in MySQL.

   .. code-block::

      insert into cdc_order values
      ('202103241000000001','webShop','2021-03-24 10:00:00','100.00','100.00','2021-03-24 10:02:03','0001','Alice','330106'),
      ('202103241606060001','appShop','2021-03-24 16:06:06','200.00','180.00','2021-03-24 16:10:06','0001','Alice','330106');

      delete from cdc_order  where order_channel = 'webShop';

      insert into cdc_order values('202103251202020001','miniAppShop','2021-03-25 12:02:02','60.00','60.00','2021-03-25 12:03:00','0002','Bob','330110');

#. Perform the following operations to view the data result in the **taskmanager.out** file:

   a. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   b. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
   c. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **taskmanager.out** file, and view result logs.

   The data result is as follows:

   .. code-block::

      +I(202103241000000001,webShop,2021-03-2410:00:00,100.0,100.0,2021-03-2410:02:03,0001,Alice,330106)
      +I(202103241606060001,appShop,2021-03-2416:06:06,200.0,180.0,2021-03-2416:10:06,0001,Alice,330106)
      -D(202103241000000001,webShop,2021-03-2410:00:00,100.0,100.0,2021-03-2410:02:03,0001,Alice,330106)
      +I(202103251202020001,miniAppShop,2021-03-2512:02:02,60.0,60.0,2021-03-2512:03:00,0002,Bob,330110)

.. _dli_08_0387__en-us_topic_0000001262815682_section09412772314:

FAQ
---

Q: How do I perform window aggregation if the MySQL CDC source table does not support definition of watermarks?

A: You can use the non-window aggregation method. That is, convert the time field into a window value, and then use **GROUP BY** to perform aggregation based on the window value.

For example, you can use the following script to collect statistics on the number of orders per minute (**order_time** indicates the order time, in the string format):

.. code-block::

   insert into printSink select DATE_FORMAT(order_time, 'yyyy-MM-dd HH:mm'), count(*) from mysqlCdcSource group by DATE_FORMAT(order_time, 'yyyy-MM-dd HH:mm');
