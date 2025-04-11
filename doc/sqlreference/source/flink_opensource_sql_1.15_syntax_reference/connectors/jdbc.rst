:original_name: dli_08_15057.html

.. _dli_08_15057:

JDBC
====

Function
--------

The JDBC connector is provided by Apache Flink and can be used to read data from and write data to common databases, such as MySQL and PostgreSQL. Source tables, result tables, and dimension tables are supported.

.. table:: **Table 1** Supported types

   ===================== ===============================================
   Type                  Description
   ===================== ===============================================
   Supported Table Types Source table, dimension table, and result table
   ===================== ===============================================

Prerequisites
-------------

-  An enhanced datasource connection with the database has been established, so that you can configure security group rules as required.

Caveats
-------

-  The JDBC sink operates in upsert mode for exchanging UPDATE/DELETE messages with the external system if a primary key is defined on the DDL, otherwise, it operates in append mode and does not support to consume UPDATE/DELETE messages.
-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.
-  Storing authentication credentials such as usernames and passwords in code or plaintext poses significant security risks. It is recommended using DEW to manage credentials instead. Storing encrypted credentials in configuration files or environment variables and decrypting them when needed ensures security. For details, see .

Syntax
------

.. code-block::

   create table jbdcTable (
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

Description
-----------

.. table:: **Table 2** Parameters

   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                        | Mandatory   | Default Value | Data Type   | Description                                                                                                                                                                                                                                                                                       |
   +==================================+=============+===============+=============+===================================================================================================================================================================================================================================================================================================+
   | connector                        | Yes         | None          | String      | Connector to be used. Set this parameter to **jdbc**.                                                                                                                                                                                                                                             |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | url                              | Yes         | None          | String      | Database URL                                                                                                                                                                                                                                                                                      |
   |                                  |             |               |             |                                                                                                                                                                                                                                                                                                   |
   |                                  |             |               |             | -  To connect to a MySQL database, the format is **jdbc:mysql://**\ *MySQL address*\ **:**\ *MySQL port*/*Database name*.                                                                                                                                                                         |
   |                                  |             |               |             | -  To connect to a PostgreSQL database, the format is **jdbc:postgresql://**\ *PostgreSQL address*\ **:**\ *PostgreSQL port*\ **/**\ *Database name*.                                                                                                                                             |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table-name                       | Yes         | None          | String      | Name of the table where the data will be read from the database                                                                                                                                                                                                                                   |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | driver                           | No          | None          | String      | Driver required for connecting to the database. If you do not set this parameter, the automatically extracted URL will be used.                                                                                                                                                                   |
   |                                  |             |               |             |                                                                                                                                                                                                                                                                                                   |
   |                                  |             |               |             | -  The default driver of the MySQL database is **com.mysql.jdbc.Driver**.                                                                                                                                                                                                                         |
   |                                  |             |               |             | -  The default driver of the PostgreSQL database is **org.postgresql.Driver**.                                                                                                                                                                                                                    |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | username                         | No          | None          | String      | Database authentication user name. This parameter must be configured in pair with **password**.                                                                                                                                                                                                   |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password                         | No          | None          | String      | Database authentication password. This parameter must be configured in pair with **username**.                                                                                                                                                                                                    |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connection.max-retry-timeout     | No          | 60s           | Duration    | Maximum timeout between retries. The timeout should be in second granularity and should not be smaller than 1 second.                                                                                                                                                                             |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.partition.column            | No          | None          | String      | Name of the column used to partition the input. For details, see :ref:`Partitioned Scan <dli_08_15057__section1974210303182>`.                                                                                                                                                                    |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.partition.num               | No          | None          | Integer     | Number of partitions to be created. For details, see :ref:`Partitioned Scan <dli_08_15057__section1974210303182>`.                                                                                                                                                                                |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.partition.lower-bound       | No          | None          | Integer     | Lower bound of values to be fetched for the first partition. For details, see :ref:`Partitioned Scan <dli_08_15057__section1974210303182>`.                                                                                                                                                       |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.partition.upper-bound       | No          | None          | Integer     | Upper bound of values to be fetched for the last partition. For details, see :ref:`Partitioned Scan <dli_08_15057__section1974210303182>`.                                                                                                                                                        |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.fetch-size                  | No          | 0             | Integer     | Number of rows fetched from the database each time. If this parameter is set to **0**, the SQL hint is ignored.                                                                                                                                                                                   |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.auto-commit                 | No          | true          | Boolean     | Whether each statement is committed in a transaction automatically.                                                                                                                                                                                                                               |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | lookup.cache.max-rows            | No          | None          | Integer     | Maximum number of rows in the lookup cache. When the rows exceed this value, the first item added to the cache will be marked as expired. By default, the lookup cache is not enabled. For details, see :ref:`Lookup Cache Functions <dli_08_15057__section1425410298425>`.                       |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | lookup.cache.ttl                 | No          | None          | Duration    | Maximum survival time of each record in the lookup cache. When the rows exceed this value, the first item added to the cache will be marked as expired. By default, the lookup cache is not enabled. For details, see :ref:`Lookup Cache Functions <dli_08_15057__section1425410298425>`.         |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | lookup.cache.caching-missing-key | No          | true          | Boolean     | Whether to cache empty query results. The default value is **true**. For details, see :ref:`Lookup Cache Functions <dli_08_15057__section1425410298425>`.                                                                                                                                         |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | lookup.max-retries               | No          | 3             | Integer     | Maximum number of retry attempts when a database query fails.                                                                                                                                                                                                                                     |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.buffer-flush.max-rows       | No          | 100           | Integer     | Maximum number of cached records before flushing, which can be set to **0** to disable it.                                                                                                                                                                                                        |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.buffer-flush.interval       | No          | 1s            | Duration    | The interval for flushing, after which the asynchronous thread will flush the data. Can be set to **0** to disable it. To fully handle the flush events of the cache asynchronously, **sink.buffer-flush.max-rows** can be set to **0** and an appropriate flush time interval can be configured. |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.max-retries                 | No          | 3             | Integer     | Maximum number of retries after a failed attempt to write records to the database.                                                                                                                                                                                                                |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.parallelism                 | No          | None          | Integer     | Defines the parallelism of the JDBC sink operator. By default, the parallelism is determined by the framework: using the same parallelism as the upstream chained operator.                                                                                                                       |
   +----------------------------------+-------------+---------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_08_15057__section1974210303182:

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

.. _dli_08_15057__section1425410298425:

Lookup Cache Functions
----------------------

The JDBC connector can be used as a lookup dimension table in temporal table joins, and currently only supports synchronous lookup mode.

By default, lookup cache is disabled. Therefore, all requests are sent to the external database. You can set **lookup.cache.max-rows** and **lookup.cache.ttl** to enable this feature. The main purpose of the lookup cache is to improve the performance of the JDBC connector in temporal table joins.

When the lookup cache is enabled, each process (i.e. TaskManager) will maintain a cache. Flink will first look up the cache, and only when the cache is not found will it send a request to the external database and update the cache with the returned data. When the cache hits the maximum cache rows **lookup.cache.max-rows** or when the rows exceed the maximum survival time **lookup.cache.ttl**, the first item added to the cache will be marked as expired. The records in the cache may not be the latest, and users can set **lookup.cache.ttl** to a smaller value to get better data refresh, but this may increase the number of requests sent to the database. Therefore, a balance between throughput and correctness should be maintained.

By default, Flink caches empty query results for primary keys, but you can switch this behavior by setting **lookup.cache.caching-missing-key** to **false**.

Data Type Mapping
-----------------

.. table:: **Table 3** Data type mapping

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

-  **Example 1: Use JDBC as the data source and Print as the result table to read data from an RDS MySQL database and write it into the Print result table.**

   #. Create an enhanced datasource connection in the VPC and subnet where RDS MySQL locates, and bind the connection to the required Flink elastic resource pool.

   #. Set RDS MySQL security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the RDS address. If the connection passes the test, it is bound to the queue.

   #. Log in to the RDS MySQL database, create table **orders** in the Flink database, and insert data.

      Create table **orders** in the Flink database.

      .. code-block::

         CREATE TABLE `flink`.`orders` (
             `order_id` VARCHAR(32) NOT NULL,
             `order_channel` VARCHAR(32) NULL,
             PRIMARY KEY (`order_id`)
         )   ENGINE = InnoDB
             DEFAULT CHARACTER SET = utf8mb4
             COLLATE = utf8mb4_general_ci;

      Insert data into the table.

      .. code-block::

         insert into orders(
           order_id,
           order_channel
         ) values
           ('1', 'webShop'),
           ('2', 'miniAppShop');

   #. Create a Flink OpenSource SQL job. Enter the following job script and submit the job.

      When you create a job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

      Storing authentication credentials such as usernames and passwords in code or plaintext poses significant security risks. It is recommended using DEW to manage credentials instead. Storing encrypted credentials in configuration files or environment variables and decrypting them when needed ensures security. For details, see .

      .. code-block::

         CREATE TABLE jdbcSource (
           order_id string,
           order_channel string
         ) WITH (
           'connector' = 'jdbc',
           'url' = 'jdbc:mysql://MySQLAddress:MySQLPort/flink',--flink is the database name created in RDS MySQL.
           'table-name' = 'orders',
           'username' = 'MySQLUsername',
           'password' = 'MySQLPassword',
           'scan.fetch-size' = '10',
           'scan.auto-commit' = 'true'
         );

         CREATE TABLE printSink (
           order_id string,
           order_channel string
         ) WITH (
           'connector' = 'print'
         );

         insert into printSink select * from jdbcSource;

   #. View the data result in the **taskmanager.out** file. The data result is as follows:

      .. code-block::

         +I(1,webShop)
         +I(2,miniAppShop)

-  **Example 2: Send data using the DataGen source table and output data to a MySQL database through the JDBC result table.**

   #. Create an enhanced datasource connection in the VPC and subnet where RDS MySQL locates, and bind the connection to the required Flink elastic resource pool.

   #. Set RDS MySQL security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the RDS address. If the connection passes the test, it is bound to the queue.

   #. Log in to the RDS MySQL database, create table **orders** in the Flink database, and insert data.

      Create table **orders** in the Flink database.

      .. code-block::

         CREATE TABLE `flink`.`orders` (
             `order_id` VARCHAR(32) NOT NULL,
             `order_channel` VARCHAR(32) NULL,
             PRIMARY KEY (`order_id`)
         )   ENGINE = InnoDB
             DEFAULT CHARACTER SET = utf8mb4
             COLLATE = utf8mb4_general_ci;

   #. Create a Flink OpenSource SQL job. Enter the following job script and submit the job.

      When you create a job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

      .. code-block::

         CREATE TABLE dataGenSource (
           order_id string,
           order_channel string
         ) WITH (
           'connector' = 'datagen',
           'fields.order_id.kind' = 'sequence',
           'fields.order_id.start' = '1',
           'fields.order_id.end' = '1000',
           'fields.order_channel.kind' = 'random',
           'fields.order_channel.length' = '5'
         );

         CREATE TABLE jdbcSink (
           order_id string,
           order_channel string,
           PRIMARY KEY(order_id) NOT ENFORCED
         ) WITH (
           'connector' = 'jdbc',
           'url? = 'jdbc:mysql://MySQLAddress:MySQLPort/flink',-- flink is the MySQL database where the orders table locates.
           'table-name' = 'orders',
           'username' = 'MySQLUsername',
           'password' = 'MySQLPassword',
           'sink.buffer-flush.max-rows' = '1'
         );

         insert into jdbcSink select * from dataGenSource;

   #. Run the SQL statement in the MySQL database to view data in the table:

      .. code-block::

         select * from orders;

-  **Example 3: Read data from the DataGen source table, use the JDBC table as the dimension table, and write the table information generated by both into the Print result table.**

   #. Create an enhanced datasource connection in the VPC and subnet where RDS MySQL locates, and bind the connection to the required Flink elastic resource pool.

   #. Set RDS MySQL security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the RDS address. If the connection passes the test, it is bound to the queue.

   #. Log in to the RDS MySQL database, create table **orders** in the Flink database, and insert data.

      Create table **orders** in the Flink database.

      .. code-block::

         CREATE TABLE `flink`.`orders` (
             `order_id` VARCHAR(32) NOT NULL,
             `order_channel` VARCHAR(32) NULL,
             PRIMARY KEY (`order_id`)
         )   ENGINE = InnoDB
             DEFAULT CHARACTER SET = utf8mb4
             COLLATE = utf8mb4_general_ci;

      Insert data into the table.

      .. code-block::

         insert into orders(
           order_id,
           order_channel
         ) values
           ('1', 'webShop'),
           ('2', 'miniAppShop');

   #. Create a Flink OpenSource SQL job. Enter the following job script and submit the job. This job script uses DataGen as the data source and JDBC as the dimension table to write data into the Print result table.

      When you create a job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

      .. code-block::

         CREATE TABLE dataGenSource (
           order_id string,
           order_time timestamp,
           proctime as Proctime()
         ) WITH (
           'connector' = 'datagen',
           'fields.order_id.kind' = 'sequence',
           'fields.order_id.start' = '1',
           'fields.order_id.end' = '2'
         );

         --Creating a dimension table
         CREATE TABLE jdbcTable (
           order_id string,
           order_channel string
         ) WITH (
           'connector' = 'jdbc',
           'url' = 'jdbc:mysql://JDBC address:JDBC port/flink',--flink is the name of the database where the orders table of RDS for MySQL is located.
           'table-name' = 'orders',
           'username' = 'JDBCUserName',
           'password' = 'JDBCPassWord',
           'lookup.cache.max-rows' = '100',
           'lookup.cache.ttl' = '1000',
           'lookup.cache.caching-missing-key' = 'false',
           'lookup.max-retries' = '5'
         );

         CREATE TABLE printSink (
           order_id string,
           order_time timestamp,
           order_channel string
         ) WITH (
           'connector' = 'print'
         );

         insert into
           printSink
         SELECT
           dataGenSource.order_id, dataGenSource.order_time, jdbcTable.order_channel
         from
           dataGenSource
           left join jdbcTable for system_time as of dataGenSource.proctime on dataGenSource.order_id = jdbcTable.order_id;

   #. View the data result in the **taskmanager.out** file. The data result is as follows:

      .. code-block::

         +I(1, xxx, webShop)
         +I(2, xxx, miniAppShop)

FAQ
---

None
