:original_name: dli_08_0320.html

.. _dli_08_0320:

HBase Dimension Table
=====================

Function
--------

Create a Hbase dimension table to connect to the source stream.

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

   +----------------------------------+-----------+----------------------------------------------------------------+
   | Parameter                        | Mandatory | Description                                                    |
   +==================================+===========+================================================================+
   | connector.type                   | Yes       | Connector type. Set this parameter to **hbase**.               |
   +----------------------------------+-----------+----------------------------------------------------------------+
   | connector.version                | Yes       | The value must be **1.4.3**.                                   |
   +----------------------------------+-----------+----------------------------------------------------------------+
   | connector. table-name            | Yes       | Table name in HBase                                            |
   +----------------------------------+-----------+----------------------------------------------------------------+
   | connector.zookeeper.quorum       | Yes       | ZooKeeper address                                              |
   +----------------------------------+-----------+----------------------------------------------------------------+
   | connector.zookeeper.znode.parent | No        | Root directory for ZooKeeper. The default value is **/hbase**. |
   +----------------------------------+-----------+----------------------------------------------------------------+

Example
-------

.. code-block::

   create table hbaseSource(
     id string,
     i Row<score string>
    ) with (
      'connector.type' = 'hbase',
      'connector.version' = '1.4.3',
      'connector.table-name' = 'user',
      'connector.zookeeper.quorum' = 'xxxx:2181'
    );
   create table source1(
     id string,
     name string,
     geneder string,
     age int,
     address string,
     proctime as PROCTIME()
   ) with (
     "connector.type" = "dis",
     "connector.region" = "",
     "connector.channel" = "read",
     "connector.ak" = "xxxxxx",
     "connector.sk" = "xxxxxx",
     "format.type" = 'csv'
   );

    create table hbaseSink(
     rowkey string,
     i Row<name string, geneder string, age int, address string>,
     j ROW<score string>
    ) with (
      'connector.type' = 'hbase',
      'connector.version' = '1.4.3',
      'connector.table-name' = 'score',
      'connector.write.buffer-flush.max-rows' = '1',
      'connector.zookeeper.quorum' = 'xxxx:2181'
    );
    insert into hbaseSink select d.id, ROW(name, geneder,age,address), ROW(score) from source1 as d join hbaseSource for system_time as of d.proctime as h on d.id = h.id;
