:original_name: dli_08_0403.html

.. _dli_08_0403:

GaussDB(DWS) Dimension Table
============================

Function
--------

Create a GaussDB(DWS) table to connect to source streams for wide table generation.

Prerequisites
-------------

-  Ensure that you have created a GaussDB(DWS) cluster using your account.
-  A DWS database table has been created.
-  An enhanced datasource connection has been created for DLI to connect to DWS clusters, so that jobs can run on the dedicated queue of DLI and you can set the security group rules as required.

Precautions
-----------

When you create a Flink OpenSource SQL job, set **Flink Version** to **1.12** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.

Syntax
------

::

   create table dwsSource (
     attr_name attr_type
     (',' attr_name attr_type)*
   )
   with (
     'connector' = 'gaussdb',
     'url' = '',
     'table-name' = '',
     'username' = '',
     'password' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                  | Mandatory   | Default Value | Data Types  | Description                                                                                                                                                                                                                                                |
   +============================+=============+===============+=============+============================================================================================================================================================================================================================================================+
   | connector                  | Yes         | None          | String      | Connector type. Set this parameter to **gaussdb**.                                                                                                                                                                                                         |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | url                        | Yes         | None          | String      | JDBC connection address.                                                                                                                                                                                                                                   |
   |                            |             |               |             |                                                                                                                                                                                                                                                            |
   |                            |             |               |             | If you use the gsjdbc4 driver, set the value in jdbc:postgresql://${ip}:${port}/${dbName} format.                                                                                                                                                          |
   |                            |             |               |             |                                                                                                                                                                                                                                                            |
   |                            |             |               |             | If you use the gsjdbc200 driver, set the value in jdbc:gaussdb://${ip}:${port}/${dbName} format.                                                                                                                                                           |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table-name                 | Yes         | None          | String      | Name of the table where the data will be read from the database                                                                                                                                                                                            |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | driver                     | No          | None          | String      | JDBC connection driver. The default value is **org.postgresql.Driver**.                                                                                                                                                                                    |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | username                   | No          | None          | String      | Database authentication user name. This parameter must be configured in pair with **password**.                                                                                                                                                            |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password                   | No          | None          | String      | Database authentication password. This parameter must be configured in pair with **username**.                                                                                                                                                             |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.partition.column      | No          | None          | String      | Name of the column used to partition the input                                                                                                                                                                                                             |
   |                            |             |               |             |                                                                                                                                                                                                                                                            |
   |                            |             |               |             | This parameter must be set when **scan.partition.lower-bound**, **scan.partition.upper-bound**, and **scan.partition.num** are all configured, and should not be set when other three parameters are not.                                                  |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.partition.lower-bound | No          | None          | Integer     | Lower bound of values to be fetched for the first partition                                                                                                                                                                                                |
   |                            |             |               |             |                                                                                                                                                                                                                                                            |
   |                            |             |               |             | This parameter must be set when **scan.partition.column**, **scan.partition.upper-bound**, and **scan.partition.num** are all configured, and should not be set when other three parameters are not.                                                       |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.partition.upper-bound | No          | None          | Integer     | Upper bound of values to be fetched for the last partition                                                                                                                                                                                                 |
   |                            |             |               |             |                                                                                                                                                                                                                                                            |
   |                            |             |               |             | This parameter must be set when **scan.partition.column**, **scan.partition.lower-bound**, and **scan.partition.num** are all configured, and should not be set when other three parameters are not.                                                       |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.partition.num         | No          | None          | Integer     | Number of partitions to be created                                                                                                                                                                                                                         |
   |                            |             |               |             |                                                                                                                                                                                                                                                            |
   |                            |             |               |             | This parameter must be set when **scan.partition.column**, **scan.partition.upper-bound**, and **scan.partition.upper-bound** are all configured, and should not be set when other three parameters are not.                                               |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.fetch-size            | No          | 0             | Integer     | Number of rows fetched from the database each time. The default value **0** indicates that the number of rows is not limited.                                                                                                                              |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.auto-commit           | No          | true          | Boolean     | Automatic commit flag.                                                                                                                                                                                                                                     |
   |                            |             |               |             |                                                                                                                                                                                                                                                            |
   |                            |             |               |             | It determines whether each statement is committed in a transaction automatically.                                                                                                                                                                          |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | lookup.cache.max-rows      | No          | None          | Integer     | The max number of rows of lookup cache. Caches exceeding the TTL will be expired.                                                                                                                                                                          |
   |                            |             |               |             |                                                                                                                                                                                                                                                            |
   |                            |             |               |             | Lookup cache is disabled by default.                                                                                                                                                                                                                       |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | lookup.cache.ttl           | No          | None          | Duration    | Maximum time to live (TTL) of for every rows in lookup cache. Caches exceeding the TTL will be expired. The format is {length value}{time unit label}, for example, **123ms, 321s**. The supported time units include d, h, min, s, and ms (default unit). |
   |                            |             |               |             |                                                                                                                                                                                                                                                            |
   |                            |             |               |             | Lookup cache is disabled by default.                                                                                                                                                                                                                       |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | lookup.max-retries         | No          | 3             | Integer     | Maximum retry times if lookup database failed.                                                                                                                                                                                                             |
   +----------------------------+-------------+---------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

Read data from a Kafka source table, use a GaussDB(DWS) table as the dimension table. Write wide table information generated by the source and dimension tables to a Kafka result table. The procedure is as follows:

#. Create an enhanced datasource connection in the VPC and subnet where DWS and Kafka locate, and bind the connection to the required Flink elastic resource pool.

#. Set GaussDB(DWS) and Kafka security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the DWS and Kafka address. If the connection passes the test, it is bound to the queue.

#. Connect to the GaussDB(DWS) database instance, create a table as a dimension table, and name the table **area_info**. Example SQL statements are as follows:

   .. code-block::

      create table public.area_info(
        area_id VARCHAR,
        area_province_name VARCHAR,
        area_city_name VARCHAR,
        area_county_name VARCHAR,
        area_street_name VARCHAR,
        region_name VARCHAR);

#. Connect to the database and run the following statement to insert test data into the dimension table **area_info**:

   .. code-block::

        insert into area_info
        (area_id, area_province_name, area_city_name, area_county_name, area_street_name, region_name)
        values
        ('330102', 'a1', 'b1', 'c1', 'd1', 'e1'),
        ('330106', 'a1', 'b1', 'c2', 'd2', 'e1'),
        ('330108', 'a1', 'b1', 'c3', 'd3', 'e1'),
        ('330110', 'a1', 'b1', 'c4', 'd4', 'e1');

#. Create a Flink OpenSource SQL job Enter the following job script and submit the job. The job script uses Kafka as the data source and a GaussDB(DWS) table as the dimension table. Data is output to a Kafka result table.

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
        'topic' = 'KafkaSourceTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'properties.group.id' = 'dws-order',
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
        'connector' = 'gaussdb',
        'driver' = 'org.postgresql.Driver',
        'url' = 'jdbc:gaussdb://DwsAddress:DwsPort/DwsDbName',
        'table-name' = 'area_info',
        'username' = 'DwsUserName',
        'password' = 'DwsPassword',
        'lookup.cache.max-rows' = '10000',
        'lookup.cache.ttl' = '2h'
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

#. Connect to the Kafka cluster and read data from the sink topic of Kafka. The result is as follows:

   .. code-block::

      {"order_id":"202103241606060001","order_channel":"appShop","order_time":"2021-03-24 16:06:06","pay_amount":200.0,"real_pay":180.0,"pay_time":"2021-03-24 16:10:06","user_id":"0001","user_name":"Alice","area_id":"330106","area_province_name":"a1","area_city_name":"b1","area_county_name":"c2","area_street_name":"d2","region_name":"e1"}

      {"order_id":"202103251202020001","order_channel":"miniAppShop","order_time":"2021-03-25 12:02:02","pay_amount":60.0,"real_pay":60.0,"pay_time":"2021-03-25 12:03:00","user_id":"0002","user_name":"Bob","area_id":"330110","area_province_name":"a1","area_city_name":"b1","area_county_name":"c4","area_street_name":"d4","region_name":"e1"}

      {"order_id":"202103251505050001","order_channel":"qqShop","order_time":"2021-03-25 15:05:05","pay_amount":500.0,"real_pay":400.0,"pay_time":"2021-03-25 15:10:00","user_id":"0003","user_name":"Cindy","area_id":"330108","area_province_name":"a1","area_city_name":"b1","area_county_name":"c3","area_street_name":"d3","region_name":"e1"}

FAQs
----

-  Q: What should I do if Flink job logs contain the following error information?

   .. code-block::

      java.io.IOException: unable to open JDBC writer
      ...
      Caused by: org.postgresql.util.PSQLException: The connection attempt failed.
      ...
      Caused by: java.net.SocketTimeoutException: connect timed out

   A: The datasource connection is not bound or the binding fails.

-  Q: How can I configure a GaussDB(DWS) table that is in a schema?

   A: In the following example configures the **area_info** table in the **dbuser2** schema.

   .. code-block::

      -- Create an address dimension table
      create table area_info (
          area_id string,
          area_province_name string,
          area_city_name string,
          area_county_name string,
          area_street_name string,
          region_name string
      ) WITH (
       'connector' = 'gaussdb',
        'driver' = 'org.postgresql.Driver',
        'url' = 'jdbc:postgresql://DwsAddress:DwsPort/DwsDbname',
        'table-name' = 'dbuser2.area_info',
        'username' = 'DwsUserName',
        'password' = 'DwsPassword',
        'lookup.cache.max-rows' = '10000',
        'lookup.cache.ttl' = '2h'
      );
