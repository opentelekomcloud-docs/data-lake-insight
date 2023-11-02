:original_name: dli_08_0201.html

.. _dli_08_0201:

Creating a DLI Table and Associating It with CSS
================================================

Function
--------

This statement is used to create a DLI table and associate it with an existing CSS table.

Prerequisites
-------------

Before creating a DLI table and associating it with CSS, you need to create a datasource connection. For details about operations on the management console, see

Syntax
------

::

   CREATE TABLE [IF NOT EXISTS] TABLE_NAME(
     FIELDNAME1 FIELDTYPE1,
     FIELDNAME2 FIELDTYPE2)
     USING CSS OPTIONS (
     'es.nodes'='xx',
     'resource'='type_path_in_CSS',
     'pushdown'='true',
     'strict'='false',
     'batch.size.entries'= '1000',
     'batch.size.bytes'= '1mb',
     'es.nodes.wan.only' = 'true',
     'es.mapping.id' = 'FIELDNAME');

Keyword
-------

.. table:: **Table 1** CREATE TABLE parameter description

   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                                                                                                                                                                                                             |
   +===================================+=========================================================================================================================================================================================================================================================================================================================================================================+
   | es.nodes                          | Before obtaining the CSS IP address, you need to create a datasource connection first..                                                                                                                                                                                                                                                                                 |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   | If you have created an enhanced datasource connection, you can use the internal IP address provided by CSS. The format is **IP1:PORT1,\ IP2:PORT2**.                                                                                                                                                                                                                    |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resource                          | The **resource** is used to specify the CSS datasource connection name. You can use **/index/type** to specify the resource location (for easier understanding, the **index** can be seen as **database** and **type** as **table**).                                                                                                                                   |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   | .. note::                                                                                                                                                                                                                                                                                                                                                               |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   |    -  In ES 6.X, a single index supports only one type, and the type name can be customized.                                                                                                                                                                                                                                                                            |
   |                                   |    -  In ES 7.X, a single index uses **\_doc** as the type name and cannot be customized. To access ES 7.X, set this parameter to **index**.                                                                                                                                                                                                                            |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | pushdown                          | Indicates whether the press function of CSS is enabled. The default value is set to **true**. If there are a large number of I/O transfer tables, the **pushdown** can be enabled to reduce I/Os when the **where** filtering conditions are met.                                                                                                                       |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | strict                            | Indicates whether the CSS **pushdown** is strict. The default value is set to **false**. In exact match scenarios, more I/Os are reduced than **pushdown**.                                                                                                                                                                                                             |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | batch.size.entries                | Maximum number of entries that can be inserted to a batch processing. The default value is **1000**. If the size of a single data record is so large that the number of data records in the bulk storage reaches the upper limit of the data amount of a single batch processing, the system stops storing data and submits the data based on the **batch.size.bytes**. |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | batch.size.bytes                  | Maximum amount of data in a single batch processing. The default value is 1 MB. If the size of a single data record is so small that the number of data records in the bulk storage reaches the upper limit of the data amount of a single batch processing, the system stops storing data and submits the data based on the **batch.size.entries**.                    |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | es.nodes.wan.only                 | Indicates whether to access the Elasticsearch node using only the domain name. The default value is **false**. If the original internal IP address provided by CSS is used as the **es.nodes**, you do not need to set this parameter or set it to **false**.                                                                                                           |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | es.mapping.id                     | Specifies a field whose value is used as the document ID in the Elasticsearch node.                                                                                                                                                                                                                                                                                     |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   | .. note::                                                                                                                                                                                                                                                                                                                                                               |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   |    -  The document ID in the same **/index/type** is unique. If a field that functions as a document ID has duplicate values, the document with the duplicate ID will be overwritten when the ES is inserted.                                                                                                                                                           |
   |                                   |    -  This feature can be used as a fault tolerance solution. When data is being inserted, the DLI job fails and some data has been inserted into Elasticsearch. The data is redundant. If Document id is set, the last redundant data will be overwritten when the DLI job is executed again.                                                                          |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | es.net.ssl                        | Whether to connect to the secure CSS cluster. The default value is **false**.                                                                                                                                                                                                                                                                                           |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | es.certificate.name               | Name of the datasource authentication used to connect to the secure CSS cluster. For details about how to create datasource authentication, see Datasource Authentication in the *Data Lake Insight User Guide*.                                                                                                                                                        |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. note::

   **batch.size.entries** and **batch.size.bytes** limit the number of data records and data volume respectively.

Example
-------

::

   CREATE TABLE IF NOT EXISTS dli_to_css (doc_id String, name string, age int)
     USING CSS OPTIONS (
     es.nodes 'to-css-1174404703-LzwpJEyx.datasource.com:9200',
     resource '/dli_index/dli_type',
     pushdown 'false',
     strict 'true',
     es.nodes.wan.only 'true',
     es.mapping.id 'doc_id');
