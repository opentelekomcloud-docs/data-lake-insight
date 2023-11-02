:original_name: dli_08_0405.html

.. _dli_08_0405:

JDBC Dimension Table
====================

Create a JDBC dimension table to connect to the source stream.

Prerequisites
-------------

You have created a JDBC instance for your account.

Precautions
-----------

When you create a Flink OpenSource SQL job, set **Flink Version** to **1.12** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.

Syntax
------

::

   CREATE TABLE  table_id (
     attr_name attr_type
     (',' attr_name attr_type)*
   )
     WITH (
     'connector' = 'jdbc',
     'url' = '',
     'table-name' = '',
     'driver' = '',
     'username' = '',
     'password' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter descriptions

   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                  | Mandatory             | Description                                                                                                                                                                                                                                                |
   +============================+=======================+============================================================================================================================================================================================================================================================+
   | connector                  | Yes                   | Data source type. The value is fixed to **jdbc**.                                                                                                                                                                                                          |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | url                        | Yes                   | Database URL                                                                                                                                                                                                                                               |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table-name                 | Yes                   | Name of the table where the data will be read from the database                                                                                                                                                                                            |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | driver                     | No                    | Driver required for connecting to the database. If you do not set this parameter, the automatically extracted URL will be used.                                                                                                                            |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | username                   | No                    | Database authentication user name. This parameter must be configured in pair with **password**.                                                                                                                                                            |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password                   | No                    | Database authentication password. This parameter must be configured in pair with **username**.                                                                                                                                                             |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.partition.column      | No                    | Name of the column used to partition the input                                                                                                                                                                                                             |
   |                            |                       |                                                                                                                                                                                                                                                            |
   |                            |                       | This parameter must be set when **scan.partition.lower-bound**, **scan.partition.upper-bound**, and **scan.partition.num** are all configured, and should not be set when other three parameters are not.                                                  |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.partition.lower-bound | No                    | Lower bound of values to be fetched for the first partition                                                                                                                                                                                                |
   |                            |                       |                                                                                                                                                                                                                                                            |
   |                            |                       | This parameter must be set when **scan.partition.column**, **scan.partition.upper-bound**, and **scan.partition.num** are all configured, and should not be set when other three parameters are not.                                                       |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.partition.upper-bound | No                    | Upper bound of values to be fetched for the last partition                                                                                                                                                                                                 |
   |                            |                       |                                                                                                                                                                                                                                                            |
   |                            |                       | This parameter must be set when **scan.partition.column**, **scan.partition.lower-bound**, and **scan.partition.num** are all configured, and should not be set when other three parameters are not.                                                       |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.partition.num         | No                    | Number of partitions to be created                                                                                                                                                                                                                         |
   |                            |                       |                                                                                                                                                                                                                                                            |
   |                            |                       | This parameter must be set when **scan.partition.column**, **scan.partition.upper-bound**, and **scan.partition.upper-bound** are all configured, and should not be set when other three parameters are not.                                               |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.fetch-size            | No                    | Number of rows fetched from the database each time. The default value is **0**, indicating the hint is ignored.                                                                                                                                            |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | lookup.cache.max-rows      | No                    | Maximum number of cached rows in a dimension table. If the number of cached rows exceeds the value , old data will be deleted. The value **-1** indicates that data cache disabled.                                                                        |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | lookup.cache.ttl           | No                    | Maximum time to live (TTL) of for every rows in lookup cache. Caches exceeding the TTL will be expired. The format is {length value}{time unit label}, for example, **123ms, 321s**. The supported time units include d, h, min, s, and ms (default unit). |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | lookup.max-retries         | No                    | Maximum number of attempts to obtain data from the dimension table. The default value is **3**.                                                                                                                                                            |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

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

Read data from a Kafka source table, use a JDBC table as the dimension table. Write table information generated by the source and dimension tables to a Kafka result table. The procedure is as follows:

#. Create an enhanced datasource connection in the VPC and subnet where MySQL and Kafka locate, and bind the connection to the required Flink elastic resource pool.

#. Set MySQL and Kafka security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the MySQL and Kafka address. If the connection passes the test, it is bound to the queue.

#. Connect to the MySQL database instance, create a table in the flink database as a dimension table, and name the table **area_info**. Example SQL statements are as follows:

   .. code-block::

      CREATE TABLE `flink`.`area_info` (
          `area_id` VARCHAR(32) NOT NULL,
          `area_province_name` VARCHAR(32) NOT NULL,
          `area_city_name` VARCHAR(32) NOT NULL,
          `area_county_name` VARCHAR(32) NOT NULL,
          `area_street_name` VARCHAR(32) NOT NULL,
          `region_name` VARCHAR(32) NOT NULL,
          PRIMARY KEY (`area_id`)
      )   ENGINE = InnoDB
          DEFAULT CHARACTER SET = utf8mb4
          COLLATE = utf8mb4_general_ci;

#. Connect to the MySQL database and run the following statement to insert test data into the JDBC dimension table **area_info**:

   .. code-block::

      insert into flink.area_info
        (area_id, area_province_name, area_city_name, area_county_name, area_street_name, region_name)
        values
        ('330102', 'a1', 'b1', 'c1', 'd1', 'e1'),
        ('330106', 'a1', 'b1', 'c2', 'd2', 'e1'),
        ('330108', 'a1', 'b1', 'c3', 'd3', 'e1'),  ('330110', 'a1', 'b1', 'c4', 'd4', 'e1');

#. Create a Flink OpenSource SQL job Enter the following job script and submit the job. The job script uses Kafka as the data source and a JDBC table as the dimension table. Data is output to a Kafka result table.

   When you create a job, set **Flink Version** to **1.12** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Set the values of the parameters in bold in the following script as needed.**

   ::

      CREATE TABLE orders (
        order_id string,
        order_channel string,
        order_time string,
        pay_amount double,
        real_pay double,
        pay_time string,
        user_id string,
        user_name string,
        area_id string,
        proctime as Proctime()
      ) WITH (
        'connector' = 'kafka',
        'topic' = 'KafkaSourceTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'properties.group.id' = 'jdbc-order',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'json'
      );

      -- Create an address dimension table
      create table area_info (
          area_id string,
          area_province_name string,
          area_city_name string,
          area_county_name string,
          area_street_name string,
          region_name string
      ) WITH (
        'connector' = 'jdbc',
        'url' = 'jdbc:mysql://JDBCAddress:JDBCPort/flink',--flink is the MySQL database where the area_info table locates.
        'table-name' = 'area_info',
        'username' = 'JDBCUserName',
        'password' = 'JDBCPassWord'
      );

      -- Generate a wide table based on the address dimension table containing detailed order information.
      create table order_detail(
          order_id string,
          order_channel string,
          order_time string,
          pay_amount double,
          real_pay double,
          pay_time string,
          user_id string,
          user_name string,
          area_id string,
          area_province_name string,
          area_city_name string,
          area_county_name string,
          area_street_name string,
          region_name string
      ) with (
        'connector' = 'kafka',
        'topic' = 'KafkaSinkTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'format' = 'json'
      );

      insert into order_detail
          select orders.order_id, orders.order_channel, orders.order_time, orders.pay_amount, orders.real_pay, orders.pay_time, orders.user_id, orders.user_name,
                 area.area_id, area.area_province_name, area.area_city_name, area.area_county_name,
                 area.area_street_name, area.region_name  from orders
                 left join area_info for system_time as of orders.proctime as area on orders.area_id = area.area_id;

#. Connect to the Kafka cluster and insert the following test data into the source topic in Kafka:

   .. code-block::

      {"order_id":"202103241606060001", "order_channel":"appShop", "order_time":"2021-03-24 16:06:06", "pay_amount":"200.00", "real_pay":"180.00", "pay_time":"2021-03-24 16:10:06", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103251202020001", "order_channel":"miniAppShop", "order_time":"2021-03-25 12:02:02", "pay_amount":"60.00", "real_pay":"60.00", "pay_time":"2021-03-25 12:03:00", "user_id":"0002", "user_name":"Bob", "area_id":"330110"}

      {"order_id":"202103251505050001", "order_channel":"qqShop", "order_time":"2021-03-25 15:05:05", "pay_amount":"500.00", "real_pay":"400.00", "pay_time":"2021-03-25 15:10:00", "user_id":"0003", "user_name":"Cindy", "area_id":"330108"}

#. Connect to the Kafka cluster and read data from the sink topic of Kafka.

   .. code-block::

      {"order_id":"202103241606060001","order_channel":"appShop","order_time":"2021-03-24 16:06:06","pay_amount":200.0,"real_pay":180.0,"pay_time":"2021-03-24 16:10:06","user_id":"0001","user_name":"Alice","area_id":"330106","area_province_name":"a1","area_city_name":"b1","area_county_name":"c2","area_street_name":"d2","region_name":"e1"}

      {"order_id":"202103251202020001","order_channel":"miniAppShop","order_time":"2021-03-25 12:02:02","pay_amount":60.0,"real_pay":60.0,"pay_time":"2021-03-25 12:03:00","user_id":"0002","user_name":"Bob","area_id":"330110","area_province_name":"a1","area_city_name":"b1","area_county_name":"c4","area_street_name":"d4","region_name":"e1"}

      {"order_id":"202103251505050001","order_channel":"qqShop","order_time":"2021-03-25 15:05:05","pay_amount":500.0,"real_pay":400.0,"pay_time":"2021-03-25 15:10:00","user_id":"0003","user_name":"Cindy","area_id":"330108","area_province_name":"a1","area_city_name":"b1","area_county_name":"c3","area_street_name":"d3","region_name":"e1"}

FAQs
----

None
