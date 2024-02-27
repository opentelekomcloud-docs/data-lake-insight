:original_name: dli_08_0400.html

.. _dli_08_0400:

Redis Result Table
==================

Function
--------

DLI outputs the Flink job output data to Redis. Redis is a key-value storage system that supports multiple types of data structures. It can be used in scenarios such as caching, event publish/subscribe, and high-speed queuing. Redis supports direct read/write of strings, hashes, lists, queues, and sets. Redis works with in-memory datasets and provides persistence. For more information about Redis, visit https://redis.io/.

Prerequisites
-------------

-  An enhanced datasource connection with Redis has been established, so that you can configure security group rules as required.

Precautions
-----------

-  When creating a Flink OpenSource SQL job, you need to set **Flink Version** to **1.12** on the **Running Parameters** tab of the job editing page, select **Save Job Log**, and set the OBS bucket for saving job logs.

-  If the Redis key field is not defined in the statement for creating the Redis result table, the generated UUID is used as the key.

-  To specify a key in Redis, you need to define a primary key in the Redis result table of Flink. The value of the primary key is the Redis key.

-  If the primary key defined for the Redis result table, it cannot be a composite primary key and only can be one field.

-  .. _dli_08_0400__en-us_topic_0000001309855877_li1877444315214:

   Constraints on **schema-syntax**:

   -  If **schema-syntax** is **map** or **array**, there can be only one non-primary key and it must be of the same **map** or **array** type.

   -  If **schema-syntax** is **fields-scores**, the number of non-primary keys must be an even number, and the second key of every two keys except the primary key must be of the **double** type. The **double** value is the score of the previous key. The following is an example:

      .. code-block::

         CREATE TABLE redisSink (
           order_id string,
           order_channel string,
           order_time double,
           pay_amount STRING,
           real_pay double,
           pay_time string,
           user_id double,
           user_name string,
           area_id double,
           primary key (order_id) not enforced
         ) WITH (
           'connector' = 'redis',
           'host' = 'RedisIP',
           'password' = 'RedisPassword',
           'data-type' = 'sorted-set',
           'deploy-mode' = 'master-replica',
           'schema-syntax' = 'fields-scores'
         );

-  .. _dli_08_0400__en-us_topic_0000001309855877_li817313914378:

   Restrictions on **data-type**:

   -  If **data-type** is **string**, only one non-primary key field is allowed.

   -  If **data-type** is **sorted-set** and **schema-syntax** is **fields** or **array**, **default-score** is used as the score.

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

::

   create table dwsSink (
     attr_name attr_type
     (',' attr_name attr_type)*
     (','PRIMARY KEY (attr_name) NOT ENFORCED)
   )
   with (
     'connector' = 'redis',
     'host' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                  | Mandatory   | Default Value | Data Type   | Description                                                                                                                                                                                      |
   +============================+=============+===============+=============+==================================================================================================================================================================================================+
   | connector                  | Yes         | None          | String      | Connector to be used. Set this parameter to **redis**.                                                                                                                                           |
   +----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | host                       | Yes         | None          | String      | Redis connector address.                                                                                                                                                                         |
   +----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | port                       | No          | 6379          | Integer     | Redis connector port.                                                                                                                                                                            |
   +----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password                   | No          | None          | String      | Redis authentication password.                                                                                                                                                                   |
   +----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | namespace                  | No          | None          | String      | Redis key namespace.                                                                                                                                                                             |
   |                            |             |               |             |                                                                                                                                                                                                  |
   |                            |             |               |             | For example, if the value is set to "person" and the key is "jack", the value in the Redis is person:jack.                                                                                       |
   +----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | delimiter                  | No          | :             | String      | Delimiter between the Redis key and namespace.                                                                                                                                                   |
   +----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | data-type                  | No          | hash          | String      | Redis data type. Available values are as follows:                                                                                                                                                |
   |                            |             |               |             |                                                                                                                                                                                                  |
   |                            |             |               |             | -  hash                                                                                                                                                                                          |
   |                            |             |               |             | -  list                                                                                                                                                                                          |
   |                            |             |               |             | -  set                                                                                                                                                                                           |
   |                            |             |               |             | -  sorted-set                                                                                                                                                                                    |
   |                            |             |               |             | -  string                                                                                                                                                                                        |
   |                            |             |               |             |                                                                                                                                                                                                  |
   |                            |             |               |             | For details about the constraints, see :ref:`Constraints on data-type <dli_08_0400__en-us_topic_0000001309855877_li817313914378>`.                                                               |
   +----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | schema-syntax              | No          | fields        | String      | Redis schema semantics. Available values are as follows:                                                                                                                                         |
   |                            |             |               |             |                                                                                                                                                                                                  |
   |                            |             |               |             | -  **fields**: applicable to all data types. This value indicates that multiple fields can be set and the value of each field is read when data is written.                                      |
   |                            |             |               |             | -  **fields-scores**: applicable to **sorted-set** data, indicating that each field is read as an independent score.                                                                             |
   |                            |             |               |             | -  **array**: applicable to **list**, **set**, and **sorted-set** data.                                                                                                                          |
   |                            |             |               |             | -  **array-scores**: applicable to **sorted-set** data.                                                                                                                                          |
   |                            |             |               |             | -  **map**: applicable to **hash** and **sorted-set** data.                                                                                                                                      |
   |                            |             |               |             |                                                                                                                                                                                                  |
   |                            |             |               |             | For details about the constraints, see :ref:`Constraints on schema-syntax <dli_08_0400__en-us_topic_0000001309855877_li1877444315214>`.                                                          |
   +----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | deploy-mode                | No          | standalone    | String      | Deployment mode of the Redis cluster. The value can be **standalone**, **master-replica**, or **cluster**. The default value is **standalone**.                                                  |
   |                            |             |               |             |                                                                                                                                                                                                  |
   |                            |             |               |             | For details about the setting, see the instance type description of the Redis cluster.                                                                                                           |
   +----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | retry-count                | No          | 5             | Integer     | Number of attempts to connect to the Redis cluster.                                                                                                                                              |
   +----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connection-timeout-millis  | No          | 10000         | Integer     | Maximum timeout for connecting to the Redis cluster.                                                                                                                                             |
   +----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | commands-timeout-millis    | No          | 2000          | Integer     | Maximum time for waiting for a completion response.                                                                                                                                              |
   +----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | rebalancing-timeout-millis | No          | 15000         | Integer     | Sleep time when the Redis cluster fails.                                                                                                                                                         |
   +----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | default-score              | No          | 0             | Double      | Default score when **data-type** is **sorted-set**.                                                                                                                                              |
   +----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ignore-retraction          | No          | false         | Boolean     | Whether to ignore Retract messages.                                                                                                                                                              |
   +----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | skip-null-values           | No          | true          | Boolean     | Whether null values will be skipped. If this parameter is **false**, **null** will be assigned for null values.                                                                                  |
   +----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key-ttl-mode               | No          | no-ttl        | String      | Whether the Redis sink TTL function will be enabled. The value can be **no-ttl**, **expire-msec**, **expire-at-date** or **expire-at-timestamp**.                                                |
   |                            |             |               |             |                                                                                                                                                                                                  |
   |                            |             |               |             | -  **no-ttl**: No expiration time is set.                                                                                                                                                        |
   |                            |             |               |             | -  **expire-msec**: validity period of the key. The parameter is a long string, in milliseconds.                                                                                                 |
   |                            |             |               |             | -  **expire-at-date**: Date and time when the key expires. The value is in UTC time format.                                                                                                      |
   |                            |             |               |             | -  **expire-at-timestamp**: Timestamp when the key expires.                                                                                                                                      |
   +----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key-ttl                    | No          | None          | String      | Supplementary parameter of **key-ttl-mode**. Available values are as follows:                                                                                                                    |
   |                            |             |               |             |                                                                                                                                                                                                  |
   |                            |             |               |             | -  If **key-ttl-mode** is **no-ttl**, this parameter does not need to be configured.                                                                                                             |
   |                            |             |               |             | -  If **key-ttl-mode** is **expire-msec**, set this parameter to a string that can be parsed into the Long type. For example, **5000** indicates that the key will expire in 5000 ms.            |
   |                            |             |               |             | -  If **key-ttl-mode** is **expire-at-date**, set this parameter to a date.                                                                                                                      |
   |                            |             |               |             | -  If **key-ttl-mode** is **expire-at-timestamp**, set this parameter to a timestamp, in milliseconds. For example, **1679385600000** indicates that the expiration time is 2023-03-21 16:00:00. |
   +----------------------------+-------------+---------------+-------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

In this example, data is read from the Kafka data source and written to the Redis result table. The procedure is as follows:

#. Create an enhanced datasource connection in the VPC and subnet where Redis locates, and bind the connection to the required Flink elastic resource pool.

#. Set Redis security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the Redis address. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Create a Flink OpenSource SQL job. Enter the following job script and submit the job.

   When you create a job, set **Flink Version** to **1.12** on the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

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
        area_id string
      ) WITH (
        'connector' = 'kafka',
        'topic' = '<yourTopic>',
        'properties.bootstrap.servers' = '<yourKafka>:<port>',
        'properties.group.id' = '<yourGroupId>',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'json'
      );
      --In the following redisSink table, data-type is set to default value hash, schema-syntax is fields, and order_id is defined as the primary key. Therefore, the value of this field is used as the Redis key.
      CREATE TABLE redisSink (
        order_id string,
        order_channel string,
        order_time string,
        pay_amount double,
        real_pay double,
        pay_time string,
        user_id string,
        user_name string,
        area_id string,
        primary key (order_id) not enforced
      ) WITH (
        'connector' = 'redis',
        'host' = '<yourRedis>',
        'password' = '<yourPassword>',
        'deploy-mode' = 'master-replica',
        'schema-syntax' = 'fields'
      );

      insert into redisSink select * from orders;

#. Connect to the Kafka cluster and insert the following test data into Kafka:

   .. code-block::

      {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103241606060001", "order_channel":"appShop", "order_time":"2021-03-24 16:06:06", "pay_amount":"200.00", "real_pay":"180.00", "pay_time":"2021-03-24 16:10:06", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

#. Run the following commands in Redis and view the result:

   -  Obtain the result whose key is **202103241606060001**.

      Run following command:

      .. code-block::

         HGETALL 202103241606060001

      Command output:

      .. code-block::

          1) "user_id"
          2) "0001"
          3) "user_name"
          4) "Alice"
          5) "pay_amount"
          6) "200.0"
          7) "real_pay"
          8) "180.0"
          9) "order_time"
         10) "2021-03-24 16:06:06"
         11) "area_id"
         12) "330106"
         13) "order_channel"
         14) "appShop"
         15) "pay_time"
         16) "2021-03-24 16:10:06"

   -  Obtain the result whose key is **202103241000000001**.

      Run following command:

      .. code-block::

         HGETALL 202103241000000001

      Command output:

      .. code-block::

          1) "user_id"
          2) "0001"
          3) "user_name"
          4) "Alice"
          5) "pay_amount"
          6) "100.0"
          7) "real_pay"
          8) "100.0"
          9) "order_time"
         10) "2021-03-24 10:00:00"
         11) "area_id"
         12) "330106"
         13) "order_channel"
         14) "webShop"
         15) "pay_time"
         16) "2021-03-24 10:02:03"

FAQ
---

-  Q: When data-type is **set**, why is the final result data less than the input data?

   A: This is because the input data contains duplicate data. Deduplication is performed in the Redis set, and the number of records in the result decreases.

-  Q: What should I do if Flink job logs contain the following error information?

   .. code-block::

      org.apache.flink.table.api.ValidationException: SQL validation failed. From line 1, column 40 to line 1, column 105: Parameters must be of the same type

   A: The array type is used. However, the types of fields in the array are different. You need to ensure that the types of fields in the array in Redis are the same.

-  Q: What should I do if Flink job logs contain the following error information?

   .. code-block::

      org.apache.flink.addons.redis.core.exception.RedisConnectorException: Wrong Redis schema for 'map' syntax: There should be a key (possibly) and 1 MAP non-key column.

   A: When **schema-syntax** is **map**, the table creation statement in Flink can contain only one non-primary key column, and the column type must be **map**.

-  Q: What should I do if Flink job logs contain the following error information?

   .. code-block::

      org.apache.flink.addons.redis.core.exception.RedisConnectorException: Wrong Redis schema for 'array' syntax: There should be a key (possibly) and 1 ARRAY non-key column.

   A: When **schema-syntax** is **array**, the table creation statement in Flink can contain only one non-primary key column, and the column type must be **array**.

-  Q: What is the function of **schema-syntax** since **data-type** has been set?

   A: **schema-syntax** is used to process special types, such as **map** and **array**.

   -  If it is set to **fields**, the value of each field is processed. If it is set to **array** or **map**, each element in the field is processed. For **fields**, the field value of the **map** or **array** type is directly used as a value in Redis.
   -  For **array** or **map**, each value in the array is used as a Redis value, and the field value of the map is used as the Redis value. **array-scores** is used to process the **sorted-set** data type. It indicates that two array fields are used, the first one is the value in the set, and the second one is the score. **fields-scores** is used to process the **sorted-set** data type, indicating that the score is derived from the defined field. The field of an odd number except the primary key indicates the value in the set, and its next field indicates its score. Therefore, its next field must be of the **double** type.

-  Q: If **data-type** is **hash**, what are the differences between **schema-syntax** set to **fields** and that to **map**?

   A: When **fields** is used, the field name in Flink is used as the Redis field of the hash data type, and the value of that field is used as the value of the hash data type in Redis. When **map** is used, the field key in Flink is used as the Redis field of the hash data type, and the value of that field is used as the value of the hash data type in Redis. The following is an example:

   -  For **fields**:

      #. The execution script of the Flink job is as follows:

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
              area_id string
            ) WITH (
              'connector' = 'kafka',
              'topic' = 'kafkaTopic',
              'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
              'properties.group.id' = 'GroupId',
              'scan.startup.mode' = 'latest-offset',
              'format' = 'json'
            );

            CREATE TABLE redisSink (
              order_id string,
              maptest Map<string, String>,
              primary key (order_id) not enforced
            ) WITH (
              'connector' = 'redis',
              'host' = 'RedisIP',
              'password' = 'RedisPassword',
              'deploy-mode' = 'master-replica',
              'schema-syntax' = 'fields'
            );

            insert into redisSink select order_id, Map[user_id, area_id] from orders;

      #. Connect to the Kafka cluster and insert the following test data into the Kafka topic:

         .. code-block::

            {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      #. In the Redis, the result is as follows:

         .. code-block::

            1) "maptest"
            2) "{0001=330106}"

   -  For **map**:

      #. The execution script of the Flink job is as follows:

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
              area_id string
            ) WITH (
              'connector' = 'kafka',
              'topic' = 'kafkaTopic',
              'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
              'properties.group.id' = 'GroupId',
              'scan.startup.mode' = 'latest-offset',
              'format' = 'json'
            );

            CREATE TABLE redisSink (
              order_id string,
              maptest Map<string, String>,
              primary key (order_id) not enforced
            ) WITH (
              'connector' = 'redis',
              'host' = 'RedisIP',
              'password' = 'RedisPassword',
              'deploy-mode' = 'master-replica',
              'schema-syntax' = 'map'
            );

            insert into redisSink select order_id, Map[user_id, area_id] from orders;

      #. Connect to the Kafka cluster and insert the following test data into the Kafka topic:

         .. code-block::

            {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      #. In the Redis, the result is as follows:

         .. code-block::

            1) "0001"
            2) "330106"

-  Q: If **data-type** is **list**, what are the differences between **schema-syntax** set to **fields** and that to **array**?

   A: The setting to **fields** or **array** does not result in different results. The only difference is that in the Flink table creation statement. **fields** can be multiple fields. However, **array** requires that the field is of the **array** type and the data types in the array must be the same. Therefore, **fields** are more flexible.

   -  For **fields**:

      #. The execution script of the Flink job is as follows:

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
              area_id string
            ) WITH (
              'connector' = 'kafka',
              'topic' = 'kafkaTopic',
              'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
              'properties.group.id' = 'GroupId',
              'scan.startup.mode' = 'latest-offset',
              'format' = 'json'
            );

            CREATE TABLE redisSink (
              order_id string,
              order_channel string,
              order_time string,
              pay_amount double,
              real_pay double,
              pay_time string,
              user_id string,
              user_name string,
              area_id string,
              primary key (order_id) not enforced
            ) WITH (
              'connector' = 'redis',
              'host' = 'RedisIP',
              'password' = 'RedisPassword',
              'data-type' = 'list',
              'deploy-mode' = 'master-replica',
              'schema-syntax' = 'fields'
            );

            insert into redisSink select * from orders;

      #. Connect to the Kafka cluster and insert the following test data into the Kafka topic:

         .. code-block::

            {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      #. View the result.

         Run the following command in Redis:

         .. code-block::

            LRANGE 202103241000000001 0 8

         The command output is as follows:

         .. code-block::

            1) "webShop"
            2) "2021-03-24 10:00:00"
            3) "100.0"
            4) "100.0"
            5) "2021-03-24 10:02:03"
            6) "0001"
            7) "Alice"
            8) "330106"

   -  For **array**:

      #. The execution script of the Flink job is as follows:

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
              area_id string
            ) WITH (
              'connector' = 'kafka',
              'topic' = 'kafkaTopic',
              'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
              'properties.group.id' = 'GroupId',
              'scan.startup.mode' = 'latest-offset',
              'format' = 'json'
            );

            CREATE TABLE redisSink (
              order_id string,
              arraytest Array<String>,
              primary key (order_id) not enforced
            ) WITH (
              'connector' = 'redis',
              'host' = 'RedisIP',
              'password' = 'RedisPassword',
              'data-type' = 'list',
              'deploy-mode' = 'master-replica',
              'schema-syntax' = 'array'
            );

            insert into redisSink select order_id, array[order_channel,order_time,pay_time,user_id,user_name,area_id] from orders;

      #. Connect to the Kafka cluster and insert the following test data into the Kafka topic:

         .. code-block::

            {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      #. In Redis, view the result. (The result is different from that of **fields** because data of the **double** type is not added to the table creation statement of the sink in Flink. Therefore, two values are missing. This is not caused by the difference between **fields** and **array**.)

         .. code-block::

            1) "webShop"
            2) "2021-03-24 10:00:00"
            3) "2021-03-24 10:02:03"
            4) "0001"
            5) "Alice"
            6) "330106"
