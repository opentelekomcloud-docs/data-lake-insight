:original_name: dli_08_0395.html

.. _dli_08_0395:

Elasticsearch Result Table
==========================

Function
--------

DLI outputs Flink job output data to Elasticsearch of Cloud Search Service (CSS). Elasticsearch is a popular enterprise-class Lucene-powered search server and provides the distributed multi-user capabilities. It delivers multiple functions, including full-text retrieval, structured search, analytics, aggregation, and highlighting. With Elasticsearch, you can achieve stable, reliable, real-time search. Elasticsearch applies to diversified scenarios, such as log analysis and site search.

CSS is a fully managed, distributed search service. It is fully compatible with open-source Elasticsearch and provides DLI with structured and unstructured data search, statistics, and report capabilities.

Prerequisites
-------------

-  When creating a Flink OpenSource SQL job, you need to set **Flink Version** to **1.12** on the **Running Parameters** tab of the job editing page, select **Save Job Log**, and set the OBS bucket for saving job logs.
-  You have created a cluster on CSS.
-  An enhanced datasource connection has been created for DLI to connect to CSS, so that jobs can run on the dedicated queue of DLI and you can set the security group rules as required.

Precautions
-----------

-  Currently, only CSS 7.X and later versions are supported. Version 7.6.2 is recommended.

-  ICMP must be enabled for the security group inbound rules of the CSS cluster.

-  For details about how to use data types, see section :ref:`Format <dli_08_0407>`.

-  Before submitting a Flink job, you are advised to select **Save Job Log** and set the OBS bucket for saving job logs. This helps you view logs and locate faults when the job fails to be submitted or runs abnormally.

-  The Elasticsearch sink can work in either upsert mode or append mode, depending on whether a primary key is defined.

   -  If a primary key is defined, the Elasticsearch sink works in upsert mode, which can consume queries containing UPDATE and DELETE messages.
   -  If a primary key is not defined, the Elasticsearch sink works in append mode which can only consume queries containing INSERT messages.

   In the Elasticsearch result table, the primary key is used to calculate the Elasticsearch document ID. The document ID is a string of up to 512 bytes. It cannot have spaces. The Elasticsearch result table generates a document ID string for every row by concatenating all primary key fields in the order defined in the DDL using a key delimiter specified by **document-id.key-delimiter**. Certain types are not allowed as a primary key field as they do not have a good string representation, for example, BYTES, ROW, ARRAY, and MAP. If no primary key is specified, Elasticsearch will generate a document ID automatically.

-  The Elasticsearch result table supports both static index and dynamic index.

   -  If you want to have a static index, the index option value should be a plain string, such as **myusers**, all the records will be consistently written into the **myusers** index.
   -  If you want to have a dynamic index, you can use **{field_name}** to reference a field value in the record to dynamically generate a target index. You can also use **{field_name|date_format_string}** to convert a field value of the TIMESTAMP, DATE, or TIME type into the format specified by **date_format_string**. **date_format_string** is compatible with Java's **DateTimeFormatter**. For example, if the option value is **myusers-{log_ts|yyyy-MM-dd}**, then a record with **log_ts** field value **2020-03-27 12:25:55** will be written into the **myusers-2020-03-27** index.

Syntax
------

.. code-block::

   create table esSink (
     attr_name attr_type
     (',' attr_name attr_type)*
     (','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
   )
   with (
     'connector' = 'elasticsearch-7',
     'hosts' = '',
     'index' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                           | Mandatory   | Default Value | Data Type   | Description                                                                                                                                                                                                                                                           |
   +=====================================+=============+===============+=============+=======================================================================================================================================================================================================================================================================+
   | connector                           | Yes         | None          | String      | Connector to be used. Set this parameter to **elasticsearch-7**, indicating to connect to a cluster of Elasticsearch 7.x or later.                                                                                                                                    |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | hosts                               | Yes         | None          | String      | Host name of the cluster where Elasticsearch is located. Use semicolons (;) to separate multiple host names.                                                                                                                                                          |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | index                               | Yes         | None          | String      | Elasticsearch index for every record. The index can be a static index (for example, **'myIndex'**) or a dynamic index (for example, **'index-{log_ts|yyyy-MM-dd}'**).                                                                                                 |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | username                            | No          | None          | String      | Username of the cluster where Elasticsearch locates. This parameter must be configured in pair with **password**.                                                                                                                                                     |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password                            | No          | None          | String      | Password of the cluster where Elasticsearch locates. This parameter must be configured in pair with **username**.                                                                                                                                                     |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | document-id.key-delimiter           | No          | \_            | String      | Delimiter of composite primary keys. The default value is **\_**.                                                                                                                                                                                                     |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | failure-handler                     | No          | fail          | String      | Failure handling strategy in case a request to Elasticsearch fails. Valid strategies are:                                                                                                                                                                             |
   |                                     |             |               |             |                                                                                                                                                                                                                                                                       |
   |                                     |             |               |             | -  **fail**: throws an exception if a request fails and thus causes a job failure.                                                                                                                                                                                    |
   |                                     |             |               |             | -  **ignore**: ignores failures and drops the request.                                                                                                                                                                                                                |
   |                                     |             |               |             | -  **retry-rejected**: re-adds requests that have failed due to queue capacity saturation.                                                                                                                                                                            |
   |                                     |             |               |             | -  **Custom class name**: for failure handling with an **ActionRequestFailureHandler** subclass.                                                                                                                                                                      |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.flush-on-checkpoint            | No          | true          | Boolean     | Whether to flush on checkpoint.                                                                                                                                                                                                                                       |
   |                                     |             |               |             |                                                                                                                                                                                                                                                                       |
   |                                     |             |               |             | If this parameter is set to **false**, the connector will not wait for all pending action requests to be acknowledged by Elasticsearch on checkpoints. Therefore, the connector does not provide any strong guarantees for at-least-once delivery of action requests. |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.bulk-flush.max-actions         | No          | 1000          | Interger    | Maximum number of buffered actions per bulk request. You can set this parameter to **0** to disable it.                                                                                                                                                               |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.bulk-flush.max-size            | No          | 2mb           | MemorySize  | Maximum size in memory of buffered actions per bulk request. It must be in MB granularity. You can set this parameter to **0** to disable it.                                                                                                                         |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.bulk-flush.interval            | No          | 1s            | Duration    | Interval for flushing buffered actions. You can set this parameter to **0** to disable it.                                                                                                                                                                            |
   |                                     |             |               |             |                                                                                                                                                                                                                                                                       |
   |                                     |             |               |             | Note:                                                                                                                                                                                                                                                                 |
   |                                     |             |               |             |                                                                                                                                                                                                                                                                       |
   |                                     |             |               |             | Both **sink.bulk-flush.max-size** and **sink.bulk-flush.max-actions** can be set to **0** with the flush interval set allowing for complete asynchronous processing of buffered actions.                                                                              |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.bulk-flush.backoff.strategy    | No          | DISABLED      | String      | Specifies how to perform retries if any flush actions failed due to a temporary request error. Valid strategies are:                                                                                                                                                  |
   |                                     |             |               |             |                                                                                                                                                                                                                                                                       |
   |                                     |             |               |             | -  **DISABLED**: no retry performed, that is, fail after the first request error.                                                                                                                                                                                     |
   |                                     |             |               |             | -  **CONSTANT**: wait for backoff delay between retries.                                                                                                                                                                                                              |
   |                                     |             |               |             | -  **EXPONENTIAL**: initially wait for backoff delay and increase exponentially between retries.                                                                                                                                                                      |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.bulk-flush.backoff.max-retries | No          | 8             | Integer     | Maximum number of backoff retries.                                                                                                                                                                                                                                    |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.bulk-flush.backoff.delay       | No          | 50ms          | Duration    | Delay between each backoff attempt.                                                                                                                                                                                                                                   |
   |                                     |             |               |             |                                                                                                                                                                                                                                                                       |
   |                                     |             |               |             | For **CONSTANT** backoff, this is simply the delay between each retry.                                                                                                                                                                                                |
   |                                     |             |               |             |                                                                                                                                                                                                                                                                       |
   |                                     |             |               |             | For **EXPONENTIAL** backoff, this is the initial base delay.                                                                                                                                                                                                          |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connection.max-retry-timeout        | No          | None          | Duration    | Maximum timeout between retries.                                                                                                                                                                                                                                      |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connection.path-prefix              | No          | None          | String      | Prefix string to be added to every REST communication, for example, **'/v1'**.                                                                                                                                                                                        |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | format                              | No          | json          | String      | The Elasticsearch connector supports to specify a format. The format must produce a valid JSON document. By default, the built-in JSON format is used.                                                                                                                |
   |                                     |             |               |             |                                                                                                                                                                                                                                                                       |
   |                                     |             |               |             | Refer to :ref:`Format <dli_08_0407>` for more details and format parameters.                                                                                                                                                                                          |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

In this example, data is read from the Kafka data source and written to the Elasticsearch result table. The procedure is as follows:

#. Create an enhanced datasource connection in the VPC and subnet where Elasticsearch and Kafka locate, and bind the connection to the required Flink elastic resource pool.

#. Set Elasticsearch and Kafka security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the Elasticsearch and Kafka address. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Log in to Kibana of the Elasticsearch cluster, select Dev Tools, enter and execute the following statement to create an index whose value is **orders**:

   .. code-block:: text

      PUT /orders
      {
        "settings": {
          "number_of_shards": 1
        },
          "mappings": {
            "properties": {
              "order_id": {
                "type": "text"
              },
              "order_channel": {
                "type": "text"
              },
              "order_time": {
                "type": "text"
              },
              "pay_amount": {
                "type": "double"
              },
              "real_pay": {
                "type": "double"
              },
              "pay_time": {
                "type": "text"
              },
              "user_id": {
                "type": "text"
              },
              "user_name": {
                "type": "text"
              },
              "area_id": {
                "type": "text"
              }
            }
          }
      }

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
        "format" = "json"
      );

      CREATE TABLE elasticsearchSink (
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
        'connector' = 'elasticsearch-7',
        'hosts' = 'ElasticsearchAddress:ElasticsearchPort',
        'index' = 'orders'
      );

      insert into elasticsearchSink select * from kafkaSource;

#. Connect to the Kafka cluster and insert the following test data into Kafka:

   .. code-block::

      {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103241606060001", "order_channel":"appShop", "order_time":"2021-03-24 16:06:06", "pay_amount":"200.00", "real_pay":"180.00", "pay_time":"2021-03-24 16:10:06", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

#. Enter the following statement in Kibana of the Elasticsearch cluster and view the result:

   .. code-block:: text

      GET orders/_search

   .. code-block::

      {
        "took" : 1,
        "timed_out" : false,
        "_shards" : {
          "total" : 1,
          "successful" : 1,
          "skipped" : 0,
          "failed" : 0
        },
        "hits" : {
          "total" : {
            "value" : 2,
            "relation" : "eq"
          },
          "max_score" : 1.0,
          "hits" : [
            {
              "_index" : "orders",
              "_type" : "_doc",
              "_id" : "ae7wpH4B1dV9conjpXeB",
              "_score" : 1.0,
              "_source" : {
                "order_id" : "202103241000000001",
                "order_channel" : "webShop",
                "order_time" : "2021-03-24 10:00:00",
                "pay_amount" : 100.0,
                "real_pay" : 100.0,
                "pay_time" : "2021-03-24 10:02:03",
                "user_id" : "0001",
                "user_name" : "Alice",
                "area_id" : "330106"
              }
            },
            {
              "_index" : "orders",
              "_type" : "_doc",
              "_id" : "au7xpH4B1dV9conjn3er",
              "_score" : 1.0,
              "_source" : {
                "order_id" : "202103241606060001",
                "order_channel" : "appShop",
                "order_time" : "2021-03-24 16:06:06",
                "pay_amount" : 200.0,
                "real_pay" : 180.0,
                "pay_time" : "2021-03-24 16:10:06",
                "user_id" : "0001",
                "user_name" : "Alice",
                "area_id" : "330106"
              }
            }
          ]
        }
      }
