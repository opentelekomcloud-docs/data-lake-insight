:original_name: dli_08_0397.html

.. _dli_08_0397:

JDBC Result Table
=================

Function
--------

DLI outputs the Flink job output data to RDS through the JDBC result table.

Prerequisites
-------------

-  An enhanced datasource connection with the instances has been established, so that you can configure security group rules as required.

Precautions
-----------

-  When creating a Flink OpenSource SQL job, you need to set **Flink Version** to **1.12** on the **Running Parameters** tab of the job editing page, select **Save Job Log**, and set the OBS bucket for saving job logs.
-  The connector operates in upsert mode if the primary key was defined; otherwise, the connector operates in append mode.

   -  In upsert mode, Flink will insert a new row or update the existing row according to the primary key. Flink can ensure the idempotence in this way. To guarantee the output result is as expected, it is recommended to define a primary key for the table.
   -  In append mode, Flink will interpret all records as INSERT messages. The INSERT operation may fail if a primary key or unique constraint violation happens in the underlying database.

Syntax
------

::

   create table jdbcSink (
     attr_name attr_type
     (',' attr_name attr_type)*
     (','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
   )
   with (
     'connector' = 'jdbc',
     'url' = '',
     'table-name' = '',
     'driver' = '',
     'username' = '',
     'password' = ''
   );

Parameters
----------

+----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Parameter                  | Mandatory   | Default Value | Data Type   | Description                                                                                                                                                              |
+============================+=============+===============+=============+==========================================================================================================================================================================+
| connector                  | Yes         | None          | String      | Connector to be used. Set this parameter to **jdbc**.                                                                                                                    |
+----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| url                        | Yes         | None          | String      | Database URL.                                                                                                                                                            |
+----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| table-name                 | Yes         | None          | String      | Name of the table where the data will be read from the database.                                                                                                         |
+----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| driver                     | No          | None          | String      | Driver required for connecting to the database. If you do not set this parameter, it will be automatically derived from the URL.                                         |
+----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| username                   | No          | None          | String      | Database authentication username. This parameter must be configured in pair with **password**.                                                                           |
+----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| password                   | No          | None          | String      | Database authentication password. This parameter must be configured in pair with **username**.                                                                           |
+----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| sink.buffer-flush.max-rows | No          | 100           | Integer     | Maximum number of rows to buffer for each write request.                                                                                                                 |
|                            |             |               |             |                                                                                                                                                                          |
|                            |             |               |             | It can improve the performance of writing data, but may increase the latency.                                                                                            |
|                            |             |               |             |                                                                                                                                                                          |
|                            |             |               |             | You can set this parameter to **0** to disable it.                                                                                                                       |
+----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| sink.buffer-flush.interval | No          | 1s            | Duration    | Interval for refreshing the buffer, during which data is refreshed by asynchronous threads.                                                                              |
|                            |             |               |             |                                                                                                                                                                          |
|                            |             |               |             | It can improve the performance of writing data, but may increase the latency.                                                                                            |
|                            |             |               |             |                                                                                                                                                                          |
|                            |             |               |             | You can set this parameter to **0** to disable it.                                                                                                                       |
|                            |             |               |             |                                                                                                                                                                          |
|                            |             |               |             | Note: If **sink.buffer-flush.max-rows** is set to **0** and the buffer refresh interval is configured, the buffer is asynchronously refreshed.                           |
|                            |             |               |             |                                                                                                                                                                          |
|                            |             |               |             | The format is *{length value}{time unit label}*, for example, **123ms, 321s**. The supported time units include **d**, **h**, **min**, **s**, and **ms** (default unit). |
+----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| sink.max-retries           | No          | 3             | Integer     | Maximum number of retries if writing records to the database failed.                                                                                                     |
+----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Data Type Mapping
-----------------

.. table:: **Table 1** Data type mapping

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

In this example, Kafka is used to send data, and Kafka data is written to the MySQL database through the JDBC result table.

#. Create an enhanced datasource connection in the VPC and subnet where MySQL and Kafka locate, and bind the connection to the required Flink elastic resource pool.

#. Set MySQL and Kafka security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the MySQL and Kafka address. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Log in to the MySQL database and create table **orders** in database **flink**.

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

#. Create a Flink OpenSource SQL job. Enter the following job script and submit the job.

   When you create a job, set **Flink Version** to **1.12** on the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

   .. code-block::

      CREATE TABLE kafkaSource (
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
        'connector' = 'kafka',
        'topic' = 'KafkaTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'json'
      );

      CREATE TABLE jdbcSink (
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
        'url? = 'jdbc:mysql://MySQLAddress:MySQLPort/flink',-- flink is the MySQL database where the orders table locates.
        'table-name' = 'orders',
        'username' = 'MySQLUsername',
        'password' = 'MySQLPassword',
        'sink.buffer-flush.max-rows' = '1'
      );

      insert into jdbcSink select * from kafkaSource;

#. Connect to the Kafka cluster and send the following test data to the Kafka topics:

   .. code-block::

      {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103241606060001", "order_channel":"appShop", "order_time":"2021-03-24 16:06:06", "pay_amount":"200.00", "real_pay":"180.00", "pay_time":"2021-03-24 16:10:06", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

#. Run the SQL statement in the MySQL database to view data in the table:

   .. code-block::

      select * from orders;

   The following is an example of the result (note that the following data is replicated from the MySQL database but not the data style in the MySQL database):

   .. code-block::

      202103241000000001,webShop,2021-03-24 10:00:00,100.0,100.0,2021-03-24 10:02:03,0001,Alice,330106
      202103241606060001,appShop,2021-03-24 16:06:06,200.0,180.0,2021-03-24 16:10:06,0001,Alice,330106

FAQ
---

None
