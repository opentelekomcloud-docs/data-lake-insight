:original_name: dli_08_0406.html

.. _dli_08_0406:

Redis Dimension Table
=====================

Function
--------

Create a Redis table to connect to source streams for wide table generation.

Prerequisites
-------------

-  An enhanced datasource connection with Redis has been established, so that you can configure security group rules as required.

Precautions
-----------

-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.12** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.

-  To obtain the key values, you can set the primary key in Flink. The primary key maps to the Redis key.

-  If the primary key cannot be a composite primary key, and only can be one field.

-  .. _dli_08_0406__en-us_topic_0000001262655738_li1877444315214:

   Constraints on **schema-syntax**:

   -  If **schema-syntax** is **map** or **array**, there can be only one non-primary key and it must be of the same **map** or **array** type.

   -  If **schema-syntax** is **fields-scores**, the number of non-primary keys must be an even number, and the second key of every two keys except the primary key must be of the **double** type. The **double** value is the score of the previous key. The following is an example:

      .. code-block::

         CREATE TABLE redisSource (
           redisKey string,
           order_id string,
           score1 double,
           order_channel string,
           score2 double,
           order_time string,
           score3 double,
           pay_amount double,
           score4 double,
           real_pay double,
           score5 double,
           pay_time string,
           score6 double,
           user_id string,
           score7 double,
           user_name string,
           score8 double,
           area_id string,
           score9 double,
           primary key (redisKey) not enforced
         ) WITH (
           'connector' = 'redis',
           'host' = 'RedisIP',
           'password' = 'RedisPassword',
           'data-type' = 'sorted-set',
           'deploy-mode' = 'master-replica',
           'schema-syntax' = 'fields-scores'
         );

-  .. _dli_08_0406__en-us_topic_0000001262655738_li817313914378:

   Restrictions on **data-type**:

   -  When **data-type** is **set**, the types of non-primary keys defined in Flink must be the same.

   -  If **data-type** is **sorted-set** and **schema-syntax** is **fields** or **array**, only **sorted set** values can be read from Redis, and the **score** value cannot be read.

   -  If **data-type** is **string**, only one non-primary key field is allowed.

   -  If **data-type** is **sorted-set** and **schema-syntax** is **map**, there can be only one non-primary key in addition to the primary key and the non-primary key must be of the **map** type. The **map** values of the non-primary key must be of the **double** type, indicating the score. The keys in the map are the values in the Redis set.

   -  If **data-type** is **sorted-set** and **schema-syntax** is **array-scores**, only two non-primary keys are allowed and must be of the **array** type.

      The first key indicates values in the Redis set. The second key is of the **array<double>** type, indicating index scores. The following is an example:

      .. code-block::

         CREATE TABLE redisSink (
           order_id string,
           arrayField Array<String>,
           arrayScore array<double>,
           primary key (order_id) not enforced
         ) WITH (
           'connector' = 'redis',
           'host' = 'RedisIP',
           'password' = 'RedisPassword',
           'data-type' = 'sorted-set',
           "default-score" = '3',
           'deploy-mode' = 'master-replica',
           'schema-syntax' = 'array-scores'
         );

Syntax
------

.. code-block::

   create table dwsSource (
     attr_name attr_type
     (',' attr_name attr_type)*
     (',' watermark for rowtime_column_name as watermark-strategy_expression)
     ,PRIMARY KEY (attr_name, ...) NOT ENFORCED
   )
   with (
     'connector' = 'redis',
     'host' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +----------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                  | Mandatory   | Default Value | Data Types  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                               |
   +============================+=============+===============+=============+===========================================================================================================================================================================================================================================================================================================================================================================================================================================================+
   | connector                  | Yes         | None          | String      | Connector type. Set this parameter to **redis**.                                                                                                                                                                                                                                                                                                                                                                                                          |
   +----------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | host                       | Yes         | None          | String      | Redis connector address                                                                                                                                                                                                                                                                                                                                                                                                                                   |
   +----------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | port                       | No          | 6379          | Integer     | Redis connector port                                                                                                                                                                                                                                                                                                                                                                                                                                      |
   +----------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password                   | No          | None          | String      | Redis authentication password                                                                                                                                                                                                                                                                                                                                                                                                                             |
   +----------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | namespace                  | No          | None          | String      | Redis key namespace                                                                                                                                                                                                                                                                                                                                                                                                                                       |
   +----------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | delimiter                  | No          | :             | String      | Delimiter between the Redis key and namespace                                                                                                                                                                                                                                                                                                                                                                                                             |
   +----------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | data-type                  | No          | hash          | String      | Redis data type. Available values are as follows:                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                            |             |               |             | -  hash                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
   |                            |             |               |             | -  list                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
   |                            |             |               |             | -  set                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
   |                            |             |               |             | -  sorted-set                                                                                                                                                                                                                                                                                                                                                                                                                                             |
   |                            |             |               |             | -  string                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
   |                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                            |             |               |             | For details about the constraints, see :ref:`Constraints on data-type <dli_08_0406__en-us_topic_0000001262655738_li817313914378>`.                                                                                                                                                                                                                                                                                                                        |
   +----------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | schema-syntax              | No          | fields        | String      | Redis schema semantics. Available values are as follows:                                                                                                                                                                                                                                                                                                                                                                                                  |
   |                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                            |             |               |             | -  **fields**: applicable to all data types                                                                                                                                                                                                                                                                                                                                                                                                               |
   |                            |             |               |             | -  **fields-scores**: applicable to **sorted set** data                                                                                                                                                                                                                                                                                                                                                                                                   |
   |                            |             |               |             | -  **array**: applicable to **list**, **set**, and **sorted set** data                                                                                                                                                                                                                                                                                                                                                                                    |
   |                            |             |               |             | -  **array-scores**: applicable to **sorted set** data                                                                                                                                                                                                                                                                                                                                                                                                    |
   |                            |             |               |             | -  **map**: applicable to **hash** and **sorted set** data                                                                                                                                                                                                                                                                                                                                                                                                |
   |                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                            |             |               |             | For details about the constraints, see :ref:`Constraints on schema-syntax <dli_08_0406__en-us_topic_0000001262655738_li1877444315214>`.                                                                                                                                                                                                                                                                                                                   |
   +----------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | deploy-mode                | No          | standalone    | String      | Deployment mode of the Redis cluster. The value can be **standalone**, **master-replica**, or **cluster**. The default value is **standalone**.                                                                                                                                                                                                                                                                                                           |
   +----------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | retry-count                | Yes         | 5             | Integer     | Size of each connection request queue. If the number of connection requests in a queue exceeds the queue size, command calling will cause RedisException. Setting **requestQueueSize** to a small value will cause exceptions to occur earlier during overload or disconnection. A larger value indicates more time required to reach the boundary, but more requests may be queued and more heap space may be used. The default value is **2147483647**. |
   +----------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connection-timeout-millis  | No          | 10000         | Integer     | Maximum timeout for connecting to the Redis cluster                                                                                                                                                                                                                                                                                                                                                                                                       |
   +----------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | commands-timeout-millis    | No          | 2000          | Integer     | Maximum time for waiting for a completion response                                                                                                                                                                                                                                                                                                                                                                                                        |
   +----------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | rebalancing-timeout-millis | No          | 15000         | Integer     | Sleep time when the Redis cluster fails                                                                                                                                                                                                                                                                                                                                                                                                                   |
   +----------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan-keys-count            | No          | 1000          | Integer     | Number of data records read in each scan                                                                                                                                                                                                                                                                                                                                                                                                                  |
   +----------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | default-score              | No          | 0             | Double      | Default score when **data-type** is **sorted-set**                                                                                                                                                                                                                                                                                                                                                                                                        |
   +----------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | deserialize-error-policy   | No          | fail-job      | Enum        | How to process a data parsing failure                                                                                                                                                                                                                                                                                                                                                                                                                     |
   |                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                            |             |               |             | Available values are as follows:                                                                                                                                                                                                                                                                                                                                                                                                                          |
   |                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                            |             |               |             | -  **fail-job**: Fail the job                                                                                                                                                                                                                                                                                                                                                                                                                             |
   |                            |             |               |             | -  **skip-row**: Skip the current data.                                                                                                                                                                                                                                                                                                                                                                                                                   |
   |                            |             |               |             | -  **null-field**: Set the current data to null.                                                                                                                                                                                                                                                                                                                                                                                                          |
   +----------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | skip-null-values           | No          | true          | Boolean     | Whether null values will be skipped                                                                                                                                                                                                                                                                                                                                                                                                                       |
   +----------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | lookup.async               | No          | false         | Boolean     | Whether asynchronous I/O will be used when this table is used as a dimension table                                                                                                                                                                                                                                                                                                                                                                        |
   +----------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

Read data from a Kafka source table, use a Redis table as the dimension table. Write wide table information generated by the source and dimension tables to a Kafka result table. The procedure is as follows:

#. Create an enhanced datasource connection in the VPC and subnet where Redis and Kafka locates, and bind the connection to the required Flink elastic resource pool.

#. Set Redis and Kafka security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the Redis address. If the connection passes the test, it is bound to the queue.

#. Run the following commands on the Redis client to send data to Redis:

   .. code-block::

      HMSET 330102  area_province_name a1 area_province_name b1 area_county_name c1 area_street_name d1 region_name e1

      HMSET 330106  area_province_name a1 area_province_name b1 area_county_name c2 area_street_name d2 region_name e1

      HMSET 330108  area_province_name a1 area_province_name b1 area_county_name c3 area_street_name d3 region_name e1

      HMSET 330110  area_province_name a1 area_province_name b1 area_county_name c4 area_street_name d4 region_name e1

#. Create a Flink OpenSource SQL job Enter the following job script and submit the job. The job script uses Kafka as the data source and a Redis table as the dimension table. Data is output to a Kafka result table.

   When you create a job, set **Flink Version** to **1.12** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Set the values of the parameters in bold in the following script as needed.**

   .. code-block::

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
        'topic' = 'kafkaSourceTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'properties.group.id' = 'GroupId',
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
          region_name string,
          primary key (area_id) not enforced -- Redis key
      ) WITH (
        'connector' = 'redis',
        'host' = 'RedisIP',
        'password' = 'RedisPassword',
        'data-type' = 'hash',
        'deploy-mode' = 'master-replica'
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
        'topic' = 'kafkaSinkTopic',
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

#. Connect to the Kafka cluster and read data from the sink topic of Kafka. The result data is as follows:

   .. code-block::

      {"order_id":"202103241606060001","order_channel":"appShop","order_time":"2021-03-24 16:06:06","pay_amount":200.0,"real_pay":180.0,"pay_time":"2021-03-24 16:10:06","user_id":"0001","user_name":"Alice","area_id":"330106","area_province_name":"a1","area_city_name":"b1","area_county_name":"c2","area_street_name":"d2","region_name":"e1"}

      {"order_id":"202103251202020001","order_channel":"miniAppShop","order_time":"2021-03-25 12:02:02","pay_amount":60.0,"real_pay":60.0,"pay_time":"2021-03-25 12:03:00","user_id":"0002","user_name":"Bob","area_id":"330110","area_province_name":"a1","area_city_name":"b1","area_county_name":"c4","area_street_name":"d4","region_name":"e1"}

      {"order_id":"202103251505050001","order_channel":"qqShop","order_time":"2021-03-25 15:05:05","pay_amount":500.0,"real_pay":400.0,"pay_time":"2021-03-25 15:10:00","user_id":"0003","user_name":"Cindy","area_id":"330108","area_province_name":"a1","area_city_name":"b1","area_county_name":"c3","area_street_name":"d3","region_name":"e1"}
