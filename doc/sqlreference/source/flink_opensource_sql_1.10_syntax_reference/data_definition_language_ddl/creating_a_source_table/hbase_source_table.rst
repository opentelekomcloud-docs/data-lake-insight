:original_name: dli_08_0306.html

.. _dli_08_0306:

HBase Source Table
==================

Function
--------

Create a source stream to obtain data from HBase as input for jobs. HBase is a column-oriented distributed cloud storage system that features enhanced reliability, excellent performance, and elastic scalability. It applies to the storage of massive amounts of data and distributed computing. You can use HBase to build a storage system capable of storing TB- or even PB-level data. With HBase, you can filter and analyze data with ease and get responses in milliseconds, rapidly mining data value. DLI can read data from HBase for filtering, analysis, and data dumping.

Prerequisites
-------------

-  An enhanced datasource connection has been created for DLI to connect to HBase, so that jobs can run on the dedicated queue of DLI and you can set the security group rules as required.

-  **If MRS HBase is used, IP addresses of all hosts in the MRS cluster have been added to host information of the enhanced datasource connection.**

   .

   For details, see section "Modifying the Host Information" in the *Data Lake Insight User Guide*.

Syntax
------

.. code-block::

   create table hbaseSource (
     attr_name attr_type
     (',' attr_name attr_type)*
     (',' watermark for rowtime_column_name as watermark-strategy_expression)
   )
   with (
     'connector.type' = 'hbase',
     'connector.version' = '1.4.3',
     'connector.table-name' = '',
     'connector.zookeeper.quorum' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +----------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                        | Mandatory             | Description                                                                                                                                                                                                                                                                                                              |
   +==================================+=======================+==========================================================================================================================================================================================================================================================================================================================+
   | connector.type                   | Yes                   | Connector type. Set this parameter to **hbase**.                                                                                                                                                                                                                                                                         |
   +----------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.version                | Yes                   | The value must be **1.4.3**.                                                                                                                                                                                                                                                                                             |
   +----------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector. table-name            | Yes                   | HBase table name                                                                                                                                                                                                                                                                                                         |
   +----------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.zookeeper.quorum       | Yes                   | ZooKeeper address                                                                                                                                                                                                                                                                                                        |
   +----------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.zookeeper.znode.parent | No                    | Root directory for ZooKeeper. The default value is **/hbase**.                                                                                                                                                                                                                                                           |
   +----------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.rowkey                 | No                    | Content of a compound rowkey to be assigned. The content is assigned to a new field based on the configuration.                                                                                                                                                                                                          |
   |                                  |                       |                                                                                                                                                                                                                                                                                                                          |
   |                                  |                       | Example: rowkey1:3,rowkey2:3,...                                                                                                                                                                                                                                                                                         |
   |                                  |                       |                                                                                                                                                                                                                                                                                                                          |
   |                                  |                       | The value 3 indicates the first three bytes of the field. The number cannot be greater than the byte size of the field and cannot be less than 1. **rowkey1:3,rowkey2:3** indicates that the first three bytes of the compound rowkey are assigned to **rowkey1**, and the last three bytes are assigned to **rowkey2**. |
   +----------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

.. code-block::

   create table hbaseSource(
     rowkey1 string,
     rowkey2 string,
     info Row<owner string>,
     car ROW<miles string, speed string>
    ) with (
      'connector.type' = 'hbase',
      'connector.version' = '1.4.3',
      'connector.table-name' = 'carinfo',
      'connector.rowkey' = 'rowkey1:1,rowkey2:3',
      'connector.zookeeper.quorum' = 'xxxx:2181'
    );
