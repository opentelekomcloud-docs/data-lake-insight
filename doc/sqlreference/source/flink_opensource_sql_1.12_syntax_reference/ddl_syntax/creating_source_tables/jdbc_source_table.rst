:original_name: dli_08_0385.html

.. _dli_08_0385:

JDBC Source Table
=================

Function
--------

The JDBC connector is a Flink's built-in connector to read data from a database.

Prerequisites
-------------

-  An enhanced datasource connection with the instances has been established, so that you can configure security group rules as required.

Precautions
-----------

When creating a Flink OpenSource SQL job, you need to set **Flink Version** to **1.12** on the **Running Parameters** tab of the job editing page, select **Save Job Log**, and set the OBS bucket for saving job logs.

Syntax
------

.. code-block::

   create table jbdcSource (
     attr_name attr_type
     (',' attr_name attr_type)*
     (','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
     (',' watermark for rowtime_column_name as watermark-strategy_expression)
   ) with (
     'connector' = 'jdbc',
     'url' = '',
     'table-name' = '',
     'username' = '',
     'password' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +----------------------------+-----------+---------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                  | Mandatory | Default Value | Data Type | Description                                                                                                                                                             |
   +============================+===========+===============+===========+=========================================================================================================================================================================+
   | connector                  | Yes       | None          | String    | Connector to be used. Set this parameter to **jdbc**.                                                                                                                   |
   +----------------------------+-----------+---------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | url                        | Yes       | None          | String    | Database URL.                                                                                                                                                           |
   +----------------------------+-----------+---------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table-name                 | Yes       | None          | String    | Name of the table where the data will be read from the database.                                                                                                        |
   +----------------------------+-----------+---------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | driver                     | No        | None          | String    | Driver required for connecting to the database. If you do not set this parameter, it will be automatically derived from the URL.                                        |
   +----------------------------+-----------+---------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | username                   | No        | None          | String    | Database authentication username. This parameter must be configured in pair with **password**.                                                                          |
   +----------------------------+-----------+---------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password                   | No        | None          | String    | Database authentication password. This parameter must be configured in pair with **username**.                                                                          |
   +----------------------------+-----------+---------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.partition.column      | No        | None          | String    | Name of the column used to partition the input. For details, see :ref:`Partitioned Scan <dli_08_0385__en-us_topic_0000001310015801_section1974210303182>`.              |
   +----------------------------+-----------+---------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.partition.num         | No        | None          | Integer   | Number of partitions to be created. For details, see :ref:`Partitioned Scan <dli_08_0385__en-us_topic_0000001310015801_section1974210303182>`.                          |
   +----------------------------+-----------+---------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.partition.lower-bound | No        | None          | Integer   | Lower bound of values to be fetched for the first partition. For details, see :ref:`Partitioned Scan <dli_08_0385__en-us_topic_0000001310015801_section1974210303182>`. |
   +----------------------------+-----------+---------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.partition.upper-bound | No        | None          | Integer   | Upper bound of values to be fetched for the last partition. For details, see :ref:`Partitioned Scan <dli_08_0385__en-us_topic_0000001310015801_section1974210303182>`.  |
   +----------------------------+-----------+---------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.fetch-size            | No        | 0             | Integer   | Number of rows fetched from the database each time. If this parameter is set to **0**, the SQL hint is ignored.                                                         |
   +----------------------------+-----------+---------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.auto-commit           | No        | true          | Boolean   | Whether each statement is committed in a transaction automatically.                                                                                                     |
   +----------------------------+-----------+---------------+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_08_0385__en-us_topic_0000001310015801_section1974210303182:

Partitioned Scan
----------------

To accelerate reading data in parallel Source task instances, Flink provides the partitioned scan feature for the JDBC table. The following parameters describe how to partition the table when reading in parallel from multiple tasks.

-  **scan.partition.column**: name of the column used to partition the input. The data type of the column must be number, date, or timestamp.
-  **scan.partition.num**: number of partitions.
-  **scan.partition.lower-bound**: minimum value of the first partition.
-  **scan.partition.upper-bound**: maximum value of the last partition.

.. note::

   -  **When a table is created, the preceding partitioned scan parameters must all be specified if any of them is specified.**
   -  The **scan.partition.lower-bound** and **scan.partition.upper-bound** parameters are used to decide the partition stride instead of filtering rows in the table. All rows in the table are partitioned and returned.

Data Type Mapping
-----------------

.. table:: **Table 2** Data type mapping

   +-----------------------+------------------------------------+------------------------------------+
   | MySQL Type            | PostgreSQL Type                    | Flink SQL Type                     |
   +=======================+====================================+====================================+
   | TINYINT               | ``-``                              | TINYINT                            |
   +-----------------------+------------------------------------+------------------------------------+
   | SMALLINT              | SMALLINT                           | SMALLINT                           |
   |                       |                                    |                                    |
   | TINYINT UNSIGNED      | INT2                               |                                    |
   |                       |                                    |                                    |
   |                       | SMALLSERIAL                        |                                    |
   |                       |                                    |                                    |
   |                       | SERIAL2                            |                                    |
   +-----------------------+------------------------------------+------------------------------------+
   | INT                   | INTEGER                            | INT                                |
   |                       |                                    |                                    |
   | MEDIUMINT             | SERIAL                             |                                    |
   |                       |                                    |                                    |
   | SMALLINT UNSIGNED     |                                    |                                    |
   +-----------------------+------------------------------------+------------------------------------+
   | BIGINT                | BIGINT                             | BIGINT                             |
   |                       |                                    |                                    |
   | INT UNSIGNED          | BIGSERIAL                          |                                    |
   +-----------------------+------------------------------------+------------------------------------+
   | BIGINT UNSIGNED       | ``-``                              | DECIMAL(20, 0)                     |
   +-----------------------+------------------------------------+------------------------------------+
   | BIGINT                | BIGINT                             | BIGINT                             |
   +-----------------------+------------------------------------+------------------------------------+
   | FLOAT                 | REAL                               | FLOAT                              |
   |                       |                                    |                                    |
   |                       | FLOAT4                             |                                    |
   +-----------------------+------------------------------------+------------------------------------+
   | DOUBLE                | FLOAT8                             | DOUBLE                             |
   |                       |                                    |                                    |
   | DOUBLE PRECISION      | DOUBLE PRECISION                   |                                    |
   +-----------------------+------------------------------------+------------------------------------+
   | NUMERIC(p, s)         | NUMERIC(p, s)                      | DECIMAL(p, s)                      |
   |                       |                                    |                                    |
   | DECIMAL(p, s)         | DECIMAL(p, s)                      |                                    |
   +-----------------------+------------------------------------+------------------------------------+
   | BOOLEAN               | BOOLEAN                            | BOOLEAN                            |
   |                       |                                    |                                    |
   | TINYINT(1)            |                                    |                                    |
   +-----------------------+------------------------------------+------------------------------------+
   | DATE                  | DATE                               | DATE                               |
   +-----------------------+------------------------------------+------------------------------------+
   | TIME [(p)]            | TIME [(p)] [WITHOUT TIMEZONE]      | TIME [(p)] [WITHOUT TIMEZONE]      |
   +-----------------------+------------------------------------+------------------------------------+
   | DATETIME [(p)]        | TIMESTAMP [(p)] [WITHOUT TIMEZONE] | TIMESTAMP [(p)] [WITHOUT TIMEZONE] |
   +-----------------------+------------------------------------+------------------------------------+
   | CHAR(n)               | CHAR(n)                            | STRING                             |
   |                       |                                    |                                    |
   | VARCHAR(n)            | CHARACTER(n)                       |                                    |
   |                       |                                    |                                    |
   | TEXT                  | VARCHAR(n)                         |                                    |
   |                       |                                    |                                    |
   |                       | CHARACTER                          |                                    |
   |                       |                                    |                                    |
   |                       | VARYING(n)                         |                                    |
   |                       |                                    |                                    |
   |                       | TEXT                               |                                    |
   +-----------------------+------------------------------------+------------------------------------+
   | BINARY                | BYTEA                              | BYTES                              |
   |                       |                                    |                                    |
   | VARBINARY             |                                    |                                    |
   |                       |                                    |                                    |
   | BLOB                  |                                    |                                    |
   +-----------------------+------------------------------------+------------------------------------+
   | ``-``                 | ARRAY                              | ARRAY                              |
   +-----------------------+------------------------------------+------------------------------------+

Example
-------

This example uses JDBC as the data source and Print as the sink to read data from the RDS MySQL database and write the data to the Print result table.

#. Create an enhanced datasource connection in the VPC and subnet where RDS MySQL locates, and bind the connection to the required Flink elastic resource pool.

#. Set RDS MySQL security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the RDS address. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Log in to the RDS MySQL database, create table **orders** in the Flink database, and insert data.

   Create table **orders** in the Flink database.

   .. code-block::

      CREATE TABLE `flink`.`orders` (
          `order_id` VARCHAR(32) NOT NULL,
          `order_channel` VARCHAR(32) NULL,
          `order_time` VARCHAR(32) NULL,
          `pay_amount` DOUBLE UNSIGNED NOT NULL,
          `real_pay` DOUBLE UNSIGNED NULL,
          `pay_time` VARCHAR(32) NULL,
          `user_id` VARCHAR(32) NULL,
          `user_name` VARCHAR(32) NULL,
          `area_id` VARCHAR(32) NULL,
          PRIMARY KEY (`order_id`)
      )   ENGINE = InnoDB
          DEFAULT CHARACTER SET = utf8mb4
          COLLATE = utf8mb4_general_ci;

   Insert data into the table.

   .. code-block::

      insert into orders(
        order_id,
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

#. Create a Flink OpenSource SQL job. Enter the following job script and submit the job.

   When you create a job, set **Flink Version** to **1.12** on the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

   .. code-block::

      CREATE TABLE jdbcSource (
        order_id string,
        order_channel string,
        order_time string,
        pay_amount double,
        real_pay double,
        pay_time string,
        user_id string,
        user_name string,
        area_id string
      ) WITH (
        'connector' = 'jdbc',
        'url' = 'jdbc:mysql://MySQLAddress:MySQLPort/flink',--flink is the database name created in RDS MySQL.
        'table-name' = 'orders',
        'username' = 'MySQLUsername',
        'password' = 'MySQLPassword'
      );

      CREATE TABLE printSink (
        order_id string,
        order_channel string,
        order_time string,
        pay_amount double,
        real_pay double,
        pay_time string,
        user_id string,
        user_name string,
        area_id string
      ) WITH (
        'connector' = 'print'
      );

      insert into printSink select * from jdbcSource;

#. Perform the following operations to view the data result in the **taskmanager.out** file:

   a. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   b. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
   c. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **taskmanager.out** file, and view result logs.

   The data result is as follows:

   .. code-block::

      +I(202103241000000001,webShop,2021-03-24 10:00:00,100.0,100.0,2021-03-24 10:02:03,0001,Alice,330106)
      +I(202103251202020001,miniAppShop,2021-03-25 12:02:02,60.0,60.0,2021-03-25 12:03:00,0002,Bob,330110)

FAQ
---

None
