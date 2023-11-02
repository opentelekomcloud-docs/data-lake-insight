:original_name: dli_08_0316.html

.. _dli_08_0316:

Elasticsearch Result Table
==========================

Function
--------

DLI exports Flink job output data to Elasticsearch of Cloud Search Service (CSS). Elasticsearch is a popular enterprise-class Lucene-powered search server and provides the distributed multi-user capabilities. It delivers multiple functions, including full-text retrieval, structured search, analytics, aggregation, and highlighting. With Elasticsearch, you can achieve stable, reliable, real-time search. Elasticsearch applies to diversified scenarios, such as log analysis and site search.

CSS is a fully managed, distributed search service. It is fully compatible with open-source Elasticsearch and provides DLI with structured and unstructured data search, statistics, and report capabilities. For more information about CSS, see .

Prerequisites
-------------

-  Ensure that you have created a cluster on CSS using your account.

   If you need to access Elasticsearch using the cluster username and password, enable the security mode and disable HTTPS for the created CSS cluster.

-  In this scenario, jobs must run on the dedicated queue of DLI. Therefore, DLI must interconnect with the enhanced datasource connection that has been connected with CSS. You can also set the security group rules as required.

Precautions
-----------

-  Currently, only CSS 7.X and later versions are supported. Version 7.6.2 is recommended.
-  Do not enable the security mode for the CSS cluster if **connector.username** and **connector.password** are not configured.
-  ICMP must be enabled for the security group inbound rule of the CSS cluster.

Syntax
------

.. code-block::

   create table esSink (
     attr_name attr_type
     (',' attr_name attr_type)*
     (','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
   )
   with (
     'connector.type' = 'elasticsearch',
     'connector.version' = '7',
     'connector.hosts' = 'http://xxxx:9200',
     'connector.index' = '',
     'connector.document-type' = '',
     'update-mode' = '',
     'format.type' = 'json'
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +----------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                        | Mandatory             | Description                                                                                                                                                                                             |
   +==================================+=======================+=========================================================================================================================================================================================================+
   | connector.type                   | Yes                   | Connector type. Set this parameter to **elasticsearch**.                                                                                                                                                |
   +----------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.version                | Yes                   | Elasticsearch version                                                                                                                                                                                   |
   |                                  |                       |                                                                                                                                                                                                         |
   |                                  |                       | Currently, only version 7 can be used. That is, the value of this parameter can only be **7**.                                                                                                          |
   +----------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.hosts                  | Yes                   | Host name of the cluster where Elasticsearch locates. Use semicolons (;) to separate multiple host names. Ensure that the host name starts with **http**, for example, **http://**\ x.x.x.x\ **:9200**. |
   +----------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.index                  | Yes                   | Elasticsearch index name                                                                                                                                                                                |
   +----------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.document-type          | Yes                   | Elasticsearch type name                                                                                                                                                                                 |
   |                                  |                       |                                                                                                                                                                                                         |
   |                                  |                       | This attribute is invalid because Elasticsearch 7 uses the default **\_doc** type.                                                                                                                      |
   +----------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | update-mode                      | Yes                   | Data update mode of the sink. The value can be **append** or **upsert**.                                                                                                                                |
   +----------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.key-delimiter          | No                    | Delimiter of compound primary keys. The default value is **\_**.                                                                                                                                        |
   +----------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.key-null-literal       | No                    | Character used to replace **null** in keys.                                                                                                                                                             |
   +----------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.failure-handler        | No                    | Policy used when an Elasticsearch request fails. The default value is **fail**.                                                                                                                         |
   |                                  |                       |                                                                                                                                                                                                         |
   |                                  |                       | **fail**: An exception is thrown when the request fails and the job fails.                                                                                                                              |
   |                                  |                       |                                                                                                                                                                                                         |
   |                                  |                       | **ignore**: The failed request is ignored.                                                                                                                                                              |
   |                                  |                       |                                                                                                                                                                                                         |
   |                                  |                       | **retry-rejected**: If the request fails because the queue running the Elasticsearch node is full, the request is resent and no failure is reported.                                                    |
   |                                  |                       |                                                                                                                                                                                                         |
   |                                  |                       | **custom**: A custom policy is used.                                                                                                                                                                    |
   +----------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.failure-handler-class  | No                    | Custom processing mode used to handle a failure                                                                                                                                                         |
   +----------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.flush-on-checkpoint    | No                    | Whether the connector waits for all pending action requests to be acknowledged by Elasticsearch on checkpoints.                                                                                         |
   |                                  |                       |                                                                                                                                                                                                         |
   |                                  |                       | The default value **true** indicates that wait for all pending action requests on checkpoints. If you set this parameter to false, the connector will not wait for the requests.                        |
   +----------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.bulk-flush.max-actions | No                    | Maximum number of records that can be written in a batch                                                                                                                                                |
   +----------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.bulk-flush.max-size    | No                    | Maximum total amount of data to be written in batches. Specify the unit when you configure this parameter. The unit is MB.                                                                              |
   +----------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.bulk-flush.interval    | No                    | Update interval for batch writing. The unit is milliseconds and is not required.                                                                                                                        |
   +----------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | format.type                      | Yes                   | Data format. Currently, only JSON is supported.                                                                                                                                                         |
   +----------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.username               | No                    | Account of the cluster where Elasticsearch locates. This parameter and must be configured in pair with **connector.password**.                                                                          |
   |                                  |                       |                                                                                                                                                                                                         |
   |                                  |                       | If the account and password are used, the security mode must be enabled and HTTPS must be disabled for the created CSS cluster.                                                                         |
   +----------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.password               | No                    | Password of the cluster where Elasticsearch locates. This parameter must be configured in pair with **connector.username**.                                                                             |
   +----------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

.. code-block::

   create table sink1(
     attr1 string,
     attr2 int
   ) with (
     'connector.type' = 'elasticsearch',
     'connector.version' = '7',
     'connector.hosts' = 'http://xxxx:9200',
     'connector.index' = 'es',
     'connector.document-type' = 'one',
     'update-mode' = 'append',
     'format.type' = 'json'
   );
