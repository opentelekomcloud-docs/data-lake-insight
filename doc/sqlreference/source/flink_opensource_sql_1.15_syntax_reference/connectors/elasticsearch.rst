:original_name: dli_08_15038.html

.. _dli_08_15038:

Elasticsearch
=============

Function
--------

DLI outputs the output data of the Flink job to an index in the Elasticsearch engine of the Cloud Search Service (CSS).

Elasticsearch is a popular enterprise-class Lucene-powered search server and provides the distributed multi-user capabilities. It delivers multiple functions, including full-text retrieval, structured search, analytics, aggregation, and highlighting. With Elasticsearch, you can achieve stable, reliable, real-time search. Elasticsearch applies to diversified scenarios, such as log analysis and site search.

CSS is a fully managed, distributed search service. It is fully compatible with open-source Elasticsearch and provides DLI with structured and unstructured data search, statistics, and report capabilities.

For details, see `Elasticsearch SQL Connector <https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/connectors/table/elasticsearch/>`__.

.. table:: **Table 1** Supported types

   ====================== ==========================
   Type                   Description
   ====================== ==========================
   Supported Table Types  Result table
   Supported Data Formats :ref:`JSON <dli_08_15021>`
   ====================== ==========================

Prerequisites
-------------

-  Ensure that you have created a cluster on CSS using your account.

Caveats
-------

-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.
-  Storing authentication credentials such as usernames and passwords in code or plaintext poses significant security risks. It is recommended using DEW to manage credentials instead. Storing encrypted credentials in configuration files or environment variables and decrypting them when needed ensures security. For details, see .
-  Fields in the **with** parameter can only be enclosed in single quotes.
-  Only CSS 7.\ *X* or later clusters are currently supported.
-  If security mode is enabled and HTTPS is enabled, you need to configure the username, password, and certificate location. Note that the **hosts** field value in this scenario starts with **https**.
-  ICMP must be enabled for the security group inbound rule of the CSS cluster.
-  Fields in the **with** parameter can only be enclosed in single quotes.
-  For details about how to use data types, see section :ref:`Format <dli_08_15014>`.

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

Parameter Description
---------------------

.. table:: **Table 2** Elasticsearch result table parameters

   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                           | Mandatory   | Default Value | Data Type   | Description                                                                                                                                                                                                                                                                 |
   +=====================================+=============+===============+=============+=============================================================================================================================================================================================================================================================================+
   | connector                           | Yes         | None          | String      | Specify what connector to use. Set this parameter to **elasticsearch-7**. This indicates connecting to Elasticsearch 7.\ *x* cluster.                                                                                                                                       |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | hosts                               | Yes         | None          | String      | Host name of the cluster where Elasticsearch is located. Use semicolons (;) to separate multiple host names.                                                                                                                                                                |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | index                               | Yes         | None          | String      | Elasticsearch index for every record. The index can be a static index (for example, **'myIndex'**) or a dynamic index (for example, **'index-{log_ts|yyyy-MM-dd}'**). See the following :ref:`Dynamic Index <dli_08_15038__section4528729183919>` section for more details. |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | username                            | No          | None          | String      | Username of the cluster where Elasticsearch locates. This parameter must be configured in pair with **password**.                                                                                                                                                           |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password                            | No          | None          | String      | Password of the cluster where Elasticsearch locates. This parameter must be configured in pair with **username**.                                                                                                                                                           |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | document-id.key-delimiter           | No          | \_            | String      | Delimiter for composite keys ("_" by default), e.g., **$** would result in IDs **KEY1$KEY2$KEY3**.                                                                                                                                                                          |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | failure-handler                     | No          | fail          | String      | Failure handling strategy in case a request to Elasticsearch fails. Valid strategies are:                                                                                                                                                                                   |
   |                                     |             |               |             |                                                                                                                                                                                                                                                                             |
   |                                     |             |               |             | -  **fail**: throws an exception if a request fails and causes a job failure.                                                                                                                                                                                               |
   |                                     |             |               |             | -  **ignore**: ignores failures and drops the request.                                                                                                                                                                                                                      |
   |                                     |             |               |             | -  **retry-rejected**: Requests that failed due to saturated queue are re-added.                                                                                                                                                                                            |
   |                                     |             |               |             | -  **custom class name**: The subclass of ActionRequestFailureHandler is used to handle failures.                                                                                                                                                                           |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.flush-on-checkpoint            | No          | true          | Boolean     | Flush on checkpoint or not.                                                                                                                                                                                                                                                 |
   |                                     |             |               |             |                                                                                                                                                                                                                                                                             |
   |                                     |             |               |             | When disabled, a sink will not wait for all pending action requests to be acknowledged by Elasticsearch on checkpoints. Thus, a sink does not provide any strong guarantees for at-least-once delivery of action requests.                                                  |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.bulk-flush.max-actions         | No          | 1000          | Interger    | Maximum number of buffered actions per bulk request. You can set this parameter to **0** to disable it.                                                                                                                                                                     |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.bulk-flush.max-size            | No          | 2mb           | MemorySize  | Maximum size in memory of buffered actions per bulk request. Must be in MB granularity. Can be set to **0** to disable it.                                                                                                                                                  |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.bulk-flush.interval            | No          | 1s            | Duration    | The interval to flush buffered actions. Can be set to **0** to disable it.                                                                                                                                                                                                  |
   |                                     |             |               |             |                                                                                                                                                                                                                                                                             |
   |                                     |             |               |             | Note, both **sink.bulk-flush.max-size** and **sink.bulk-flush.max-actions** can be set to **0** with the flush interval set allowing for complete async processing of buffered actions.                                                                                     |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.bulk-flush.backoff.strategy    | No          | DISABLED      | String      | Specify how to perform retries if any flush actions failed due to a temporary request error. Valid strategies are:                                                                                                                                                          |
   |                                     |             |               |             |                                                                                                                                                                                                                                                                             |
   |                                     |             |               |             | -  **DISABLED**: no retry performed, i.e. fail after the first request error.                                                                                                                                                                                               |
   |                                     |             |               |             | -  **CONSTANT**: wait for backoff delay between retries.                                                                                                                                                                                                                    |
   |                                     |             |               |             | -  **EXPONENTIAL**: initially wait for backoff delay and increase exponentially between retries.                                                                                                                                                                            |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.bulk-flush.backoff.max-retries | No          | None          | Integer     | Maximum number of rollback retries.                                                                                                                                                                                                                                         |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.bulk-flush.backoff.delay       | No          | None          | Duration    | Delay between each backoff attempt.                                                                                                                                                                                                                                         |
   |                                     |             |               |             |                                                                                                                                                                                                                                                                             |
   |                                     |             |               |             | For **CONSTANT** backoff, this is simply the delay between each retry. For **EXPONENTIAL** backoff, this is the initial base delay.                                                                                                                                         |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connection.path-prefix              | No          | None          | String      | Prefix string added to each REST communication, for example, **'/v1'**.                                                                                                                                                                                                     |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connection.request-timeout          | No          | None          | Duration    | The timeout in milliseconds for requesting a connection from the connection manager. The timeout must be larger than or equal to 0. A timeout value of zero is interpreted as an infinite timeout.                                                                          |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connection.timeout                  | No          | None          | Duration    | The timeout in milliseconds for establishing a connection.                                                                                                                                                                                                                  |
   |                                     |             |               |             |                                                                                                                                                                                                                                                                             |
   |                                     |             |               |             | The timeout must be larger than or equal to 0. A timeout value of zero is interpreted as an infinite timeout.                                                                                                                                                               |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | socket.timeout                      | No          | None          | Duration    | The socket timeout (SO_TIMEOUT) for waiting for data. The timeout must be larger than or equal to 0. A timeout value of zero is interpreted as an infinite timeout.                                                                                                         |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | format                              | No          | json          | String      | Elasticsearch connector supports to specify a format. The format must produce a valid JSON document. By default, the built-in JSON format is used.                                                                                                                          |
   |                                     |             |               |             |                                                                                                                                                                                                                                                                             |
   |                                     |             |               |             | Refer to :ref:`Format <dli_08_15014>` for more details and format parameters.                                                                                                                                                                                               |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | certificate                         | No          | None          | String      | Location of the Elasticsearch cluster certificate in OBS.                                                                                                                                                                                                                   |
   |                                     |             |               |             |                                                                                                                                                                                                                                                                             |
   |                                     |             |               |             | This parameter is required only when the security mode and HTTPS are enabled.                                                                                                                                                                                               |
   |                                     |             |               |             |                                                                                                                                                                                                                                                                             |
   |                                     |             |               |             | Download the certificate from the CSS management console and upload the certificate to OBS. This parameter specifies the OBS address.                                                                                                                                       |
   |                                     |             |               |             |                                                                                                                                                                                                                                                                             |
   |                                     |             |               |             | Example: **obs://bucket/path/CloudSearchService.cer**                                                                                                                                                                                                                       |
   +-------------------------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Key Handling
------------

The Elasticsearch sink can work in either upsert mode or append mode, depending on whether a primary key is defined.

-  If a primary key is defined, the Elasticsearch sink works in upsert mode which can consume queries containing UPDATE/DELETE messages.
-  If a primary key is not defined, the Elasticsearch sink works in append mode which can only consume queries containing INSERT only messages.

In the Elasticsearch connector, the primary key is used to calculate the Elasticsearch document ID, which is a string of up to 512 bytes. It cannot have whitespaces.

The Elasticsearch connector generates a document ID string for every row by concatenating all primary key fields in the order defined in the DDL using a key delimiter specified by **document-id.key-delimiter**. Certain types are not allowed as a primary key field as they do not have a good string representation, e.g. **BYTES**, **ROW**, **ARRAY**, **MAP**, etc.

If no primary key is specified, Elasticsearch will generate a document ID automatically.

.. _dli_08_15038__section4528729183919:

Dynamic Index
-------------

The Elasticsearch sink supports both static index and dynamic index.

-  If you want to have a static index, the index option value should be a plain string, e.g. **myusers**, all the records will be consistently written into **myusers** index.
-  If you want to have a dynamic index, you can use **{field_name}** to reference a field value in the record to dynamically generate a target index.

   -  You can use **{field_name|date_format_string}** to convert a field value of TIMESTAMP/DATE/TIME type into the format specified by the **date_format_string**. The **date_format_string** is compatible with Java's **DateTimeFormatter**. For example, if the option value is **myusers-{log_ts|yyyy-MM-dd}**, then a record with **log_ts** field value **2020-03-27 12:25:55** will be written into **myusers-2020-03-27** index.
   -  You can use **{now()|date_format_string}** to convert the current system time to the format specified by **date_format_string**. The corresponding time type of **now()** is **TIMESTAMP_WITH_LTZ**. When formatting the system time as a string, the time zone configured in the **session** through **table.local-time-zone** will be used. You can use **NOW()**, **now()**, **CURRENT_TIMESTAMP**, or **current_timestamp**.

      .. caution::

         When using the dynamic index generated by the current system time, for changelog stream, there is no guarantee that the records with the same primary key can generate the same index name. Therefore, the dynamic index based on the system time can only support append only stream.

Example
-------

In this example, data is read from the Kafka data source and written to the Elasticsearch result table (Elasticsearch 7.10.2). The procedure is as follows:

#. Create an enhanced datasource connection in the VPC and subnet where Elasticsearch and Kafka locate, and bind the connection to the required Flink elastic resource pool.

#. Set Elasticsearch and Kafka security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the Elasticsearch and Kafka addresses. If the connection passes the test, it is bound to the queue.

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

   Change the values of the parameters in bold as needed in the following script.

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
        "took" : 201,
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
              "_id" : "fopyx4sBUuT2wThgYGcp",
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
            },
            {
              "_index" : "orders",
              "_type" : "_doc",
              "_id" : "f4pyx4sBUuT2wThgYGcr",
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
            }
          ]
        }
      }
