:original_name: dli_08_15062.html

.. _dli_08_15062:

Source Table
============

Function
--------

Create a source stream to obtain data from Redis as input for jobs.

Prerequisites
-------------

An enhanced datasource connection has been created for DLI to connect to the Redis database, so that you can configure security group rules as required.

.. _dli_08_15062__section2069551919512:

Caveats
-------

-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.

-  Storing authentication credentials such as usernames and passwords in code or plaintext poses significant security risks. It is recommended using DEW to manage credentials instead. Storing encrypted credentials in configuration files or environment variables and decrypting them when needed ensures security. For details, see .

-  To obtain the key values, you can set the primary key in Flink. The primary key maps to the Redis key.

-  The primary key cannot be a composite primary key, and only can be one field.

-  .. _dli_08_15062__li156214421364:

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

-  .. _dli_08_15062__li817313914378:

   Restrictions on **data-type**:

   -  When **data-type** is **set**, the types of non-primary keys defined in Flink must be the same.

   -  If **data-type** is **sorted-set** and **schema-syntax** is **fields** or **array**, only **sorted-set** values can be read from Redis, and the **score** value cannot be read.

   -  If **data-type** is **string**, only one non-primary key field is allowed.

   -  If **data-type** is **sorted-set** and **schema-syntax** is **map**, only one non-primary key field is allowed besides the primary key field.

      This non-primary key field must be of the **map** type. The map value of the field must be of the **double** type, indicating the score. The map key of the field indicates the value in the Redis set.

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

   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                  | Mandatory   | Default Value | Data Type   | Description                                                                                                                                                                        |
   +============================+=============+===============+=============+====================================================================================================================================================================================+
   | connector                  | Yes         | None          | String      | Connector to be used. Set this parameter to **redis**.                                                                                                                             |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | host                       | Yes         | None          | String      | Redis connector address.                                                                                                                                                           |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | port                       | No          | 6379          | Integer     | Redis connector port.                                                                                                                                                              |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password                   | No          | None          | String      | Redis authentication password.                                                                                                                                                     |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | namespace                  | No          | None          | String      | Redis key namespace.                                                                                                                                                               |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | delimiter                  | No          | :             | String      | Delimiter between the Redis key and namespace.                                                                                                                                     |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | data-type                  | No          | hash          | String      | Redis data type. Available values are as follows:                                                                                                                                  |
   |                            |             |               |             |                                                                                                                                                                                    |
   |                            |             |               |             | -  hash                                                                                                                                                                            |
   |                            |             |               |             | -  list                                                                                                                                                                            |
   |                            |             |               |             | -  set                                                                                                                                                                             |
   |                            |             |               |             | -  sorted-set                                                                                                                                                                      |
   |                            |             |               |             | -  string                                                                                                                                                                          |
   |                            |             |               |             |                                                                                                                                                                                    |
   |                            |             |               |             | For details about the constraints, see :ref:`Constraints on data-type <dli_08_15062__li817313914378>`.                                                                             |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | schema-syntax              | No          | fields        | String      | Redis schema semantics. Available values are as follows (for details, see :ref:`Caveats <dli_08_15062__section2069551919512>` and :ref:`FAQ <dli_08_15062__section831915115116>`): |
   |                            |             |               |             |                                                                                                                                                                                    |
   |                            |             |               |             | -  **fields**: applicable to all data types                                                                                                                                        |
   |                            |             |               |             | -  **fields-scores**: applicable to **sorted-set** data                                                                                                                            |
   |                            |             |               |             | -  **array**: applicable to **list**, **set**, and **sorted-set** data                                                                                                             |
   |                            |             |               |             | -  **array-scores**: applicable to **sorted-set** data                                                                                                                             |
   |                            |             |               |             | -  **map**: applicable to **hash** and **sorted-set** data                                                                                                                         |
   |                            |             |               |             |                                                                                                                                                                                    |
   |                            |             |               |             | For details about the constraints, see :ref:`Constraints on schema-syntax <dli_08_15062__li156214421364>`.                                                                         |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | deploy-mode                | No          | standalone    | String      | Deployment mode of the Redis cluster. The value can be **standalone**, **master-replica**, or **cluster**. The default value is **standalone**.                                    |
   |                            |             |               |             |                                                                                                                                                                                    |
   |                            |             |               |             | The deployment mode varies depending on the Redis instance type.                                                                                                                   |
   |                            |             |               |             |                                                                                                                                                                                    |
   |                            |             |               |             | Select **standalone** for single-node, master/standby, and Proxy Cluster instances.                                                                                                |
   |                            |             |               |             |                                                                                                                                                                                    |
   |                            |             |               |             | For a cluster instance, select **cluster**.                                                                                                                                        |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | retry-count                | No          | 5             | Integer     | Number of attempts to connect to the Redis cluster.                                                                                                                                |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connection-timeout-millis  | No          | 10000         | Integer     | Maximum timeout for connecting to the Redis cluster.                                                                                                                               |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | commands-timeout-millis    | No          | 2000          | Integer     | Maximum time for waiting for a completion response.                                                                                                                                |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | rebalancing-timeout-millis | No          | 15000         | Integer     | Sleep time when the Redis cluster fails.                                                                                                                                           |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan-keys-count            | No          | 1000          | Integer     | Number of data records read in each scan.                                                                                                                                          |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | default-score              | No          | 0             | Double      | Default score when **data-type** is **sorted-set**.                                                                                                                                |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | deserialize-error-policy   | No          | fail-job      | Enum        | Policy of how to process a data parsing failure. Available values are as follows:                                                                                                  |
   |                            |             |               |             |                                                                                                                                                                                    |
   |                            |             |               |             | -  **fail-job**: Fail the job.                                                                                                                                                     |
   |                            |             |               |             | -  **skip-row**: Skip the current data.                                                                                                                                            |
   |                            |             |               |             | -  **null-field**: Set the current data to null.                                                                                                                                   |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | skip-null-values           | No          | true          | Boolean     | Whether null values will be skipped.                                                                                                                                               |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ignore-retractions         | No          | false         | Boolean     | The connector should ignore retraction messages in the update insert/withdraw flow mode.                                                                                           |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key-column                 | No          | None          | String      | Schema key of the Redis table.                                                                                                                                                     |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | source.parallelism         | No          | None          | int         | Defines the custom parallelism of the source. By default, if this option is not defined, the parallelism from the global configuration is used.                                    |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

In this example, data is read from the DCS Redis data source and written to the Print result table. The procedure is as follows:

#. Create an enhanced datasource connection in the VPC and subnet where Redis locates, and bind the connection to the required Flink elastic resource pool.

#. Set Redis security groups and add inbound rules to allow access from the Flink queue.

   Test the connectivity using the Redis address. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Run the following commands on the Redis client to insert data into different keys and store the data in hash format:

   .. code-block::

      HMSET redisSource order_id 202103241000000001 order_channel webShop order_time "2021-03-24 10:00:00" pay_amount 100.00 real_pay 100.00 pay_time "2021-03-24 10:02:03" user_id 0001 user_name Alice area_id 330106

      HMSET redisSource1 order_id 202103241606060001 order_channel appShop order_time "2021-03-24 16:06:06" pay_amount 200.00 real_pay 180.00 pay_time "2021-03-24 16:10:06" user_id 0001 user_name Alice area_id 330106

      HMSET redisSource2 order_id 202103251202020001 order_channel miniAppShop order_time "2021-03-25 12:02:02" pay_amount 60.00 real_pay 60.00 pay_time "2021-03-25 12:03:00" user_id 0002 user_name Bob area_id 330110

#. Create a Flink OpenSource SQL job. Enter the following job script to read data in hash format from Redis.

   Change the values of the parameters in bold as needed in the following script.

   .. code-block::

      CREATE TABLE redisSource (
        redisKey string,
        order_id string,
        order_channel string,
        order_time string,
        pay_amount double,
        real_pay double,
        pay_time string,
        user_id string,
        user_name string,
        area_id string,
        primary key (redisKey) not enforced  --Obtains the key value from Redis.
      ) WITH (
        'connector' = 'redis',
        'host' = 'RedisIP',
        'password' = 'RedisPassword',
        'data-type' = 'hash',
        'deploy-mode' = 'master-replica'
      );

      CREATE TABLE printSink (
        redisKey string,
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

      insert into printSink select * from redisSource;

#. Perform the following operations to view the data result in the **taskmanager.out** file:

   a. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   b. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
   c. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **taskmanager.out** file, and view result logs.

   The data result is as follows:

   .. code-block::

      +I(redisSource1,202103241606060001,appShop,2021-03-24 16:06:06,200.0,180.0,2021-03-24 16:10:06,0001,Alice,330106)
      +I(redisSource,202103241000000001,webShop,2021-03-24 10:00:00,100.0,100.0,2021-03-24 10:02:03,0001,Alice,330106)
      +I(redisSource2,202103251202020001,miniAppShop,2021-03-25 12:02:02,60.0,60.0,2021-03-25 12:03:00,0002,Bob,330110)

.. _dli_08_15062__section831915115116:

FAQ
---

-  Q: What should I do if the Flink job execution fails and the log contains the following error information?

   .. code-block::

      Caused by: org.apache.flink.client.program.ProgramInvocationException: The main method caused an error: RealLine:36;Usage of 'set' data-type and 'fields' schema syntax in source Redis connector with multiple non-key column types. As 'set' in Redis is not sorted, it's not possible to map 'set's values to table schema with different types.

   A: If **data-type** is **set**, the data types of non-primary key fields in Flink are different. As a result, this error is reported. When **data-type** is **set**, the types of non-primary keys defined in Flink must be the same.

-  Q: If **data-type** is **hash**, what are the differences between **schema-syntax** set to **fields** and that to **map**?

   A: When **schema-syntax** is set to **fields**, the hash value in the Redis key is assigned to the field with the same name in Flink. When **schema-syntax** is set to **map**, the hash key and hash value of each hash in Redis are put into a map, which represents the value of the corresponding Flink field. Specifically, this map contains all hash keys and hash values of a key in Redis.

   -  For **fields**:

      #. Insert the following data into Redis:

         .. code-block::

            HMSET redisSource order_id 202103241000000001 order_channel webShop order_time "2021-03-24 10:00:00" pay_amount 100.00 real_pay 100.00 pay_time "2021-03-24 10:02:03" user_id 0001 user_name Alice area_id 330106

      #. When **schema-syntax** is set to **fields**, use the following job script:

         .. code-block::

            CREATE TABLE redisSource (
              redisKey string,
              order_id string,
              order_channel string,
              order_time string,
              pay_amount double,
              real_pay double,
              pay_time string,
              user_id string,
              user_name string,
              area_id string,
              primary key (redisKey) not enforced
            ) WITH (
              'connector' = 'redis',
              'host' = 'RedisIP',
              'password' = 'RedisPassword',
              'data-type' = 'hash',
              'deploy-mode' = 'master-replica'
            );

            CREATE TABLE printSink (
              redisKey string,
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

            insert into printSink select * from redisSource;

      #. The job execution result is as follows:

         .. code-block::

            +I(redisSource,202103241000000001,webShop,2021-03-24 10:00:00,100.0,100.0,2021-03-24 10:02:03,0001,Alice,330106)

   -  For **map**:

      #. Insert the following data into Redis:

         .. code-block::

            HMSET redisSource order_id 202103241000000001 order_channel webShop order_time "2021-03-24 10:00:00" pay_amount 100.00 real_pay 100.00 pay_time "2021-03-24 10:02:03" user_id 0001 user_name Alice area_id 330106

      #. When **schema-syntax** is set to **map**, use the following job script:

         .. code-block::

            CREATE TABLE redisSource (
              redisKey string,
              order_result map<string, string>,
              primary key (redisKey) not enforced
            ) WITH (
              'connector' = 'redis',
              'host' = 'RedisIP',
              'password' = 'RedisPassword',
              'data-type' = 'hash',
              'deploy-mode' = 'master-replica',
              'schema-syntax' = 'map'
            );

            CREATE TABLE printSink (
              redisKey string,
              order_result map<string, string>
            ) WITH (
              'connector' = 'print'
            );

            insert into printSink select * from redisSource;

      #. The job execution result is as follows:

         .. code-block::

            +I(redisSource,{user_id=0001, user_name=Alice, pay_amount=100.00, real_pay=100.00, order_time=2021-03-24 10:00:00, area_id=330106, order_id=202103241000000001, order_channel=webShop, pay_time=2021-03-24 10:02:03})
