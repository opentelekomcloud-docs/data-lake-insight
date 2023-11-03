:original_name: dli_08_0315.html

.. _dli_08_0315:

HBase Result Table
==================

Function
--------

DLI outputs the job data to HBase. HBase is a column-oriented distributed cloud storage system that features enhanced reliability, excellent performance, and elastic scalability. It applies to the storage of massive amounts of data and distributed computing. You can use HBase to build a storage system capable of storing TB- or even PB-level data. With HBase, you can filter and analyze data with ease and get responses in milliseconds, rapidly mining data value. Structured and semi-structured key-value data can be stored, including messages, reports, recommendation data, risk control data, logs, and orders. With DLI, you can write massive volumes of data to HBase at a high speed and with low latency.

Prerequisites
-------------

An enhanced datasource connection has been created for DLI to connect to HBase, so that jobs can run on the dedicated queue of DLI and you can set the security group rules as required.

-  **If MRS HBase is used, IP addresses of all hosts in the MRS cluster have been added to host information of the enhanced datasource connection.**

   .

Syntax
------

.. code-block::

   create table hbaseSink (
     attr_name attr_type
     (',' attr_name attr_type)*
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

   +---------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                             | Mandatory             | Description                                                                                                                                       |
   +=======================================+=======================+===================================================================================================================================================+
   | connector.type                        | Yes                   | Connector type. Set this parameter to **hbase**.                                                                                                  |
   +---------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.version                     | Yes                   | The value must be **1.4.3**.                                                                                                                      |
   +---------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.table-name                  | Yes                   | HBase table name                                                                                                                                  |
   +---------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.zookeeper.quorum            | Yes                   | ZooKeeper address                                                                                                                                 |
   +---------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.zookeeper.znode.parent      | No                    | Root directory for ZooKeeper. The default value is **/hbase**.                                                                                    |
   +---------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.write.buffer-flush.max-size | No                    | Maximum buffer size for each data write. The default value is 2 MB. The unit is MB.                                                               |
   +---------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.write.buffer-flush.max-rows | No                    | Maximum number of data records that can be updated each time                                                                                      |
   +---------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.write.buffer-flush.interval | No                    | Update time. The default value is **0s**. Example value: **2s**.                                                                                  |
   +---------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.rowkey                      | No                    | Content of a compound rowkey to be assigned. The content is assigned to a new field based on the configuration.                                   |
   |                                       |                       |                                                                                                                                                   |
   |                                       |                       | Example: rowkey1:3,rowkey2:3, ...                                                                                                                 |
   |                                       |                       |                                                                                                                                                   |
   |                                       |                       | The value 3 indicates the first three bytes of the field. The number cannot be greater than the byte size of the field and cannot be less than 1. |
   +---------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

.. code-block::

    create table hbaseSink(
     rowkey string,
     name string,
     i Row<geneder string, age int>,
     j Row<address string>
    ) with (
      'connector.type' = 'hbase',
      'connector.version' = '1.4.3',
      'connector.table-name' = 'sink',
      'connector.rowkey' = 'rowkey:1,name:3',
      'connector.write.buffer-flush.max-rows' = '5',
      'connector.zookeeper.quorum' = 'xxxx:2181'
    );
