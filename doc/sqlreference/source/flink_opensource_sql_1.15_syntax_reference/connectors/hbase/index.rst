:original_name: dli_08_15042.html

.. _dli_08_15042:

HBase
=====

The HBase connector allows for reading from and writing to an HBase cluster. This section describes how to set up the HBase Connector to run SQL queries against HBase.

HBase always works in upsert mode for exchange changelog messages with the external system using a primary key defined on the DDL. The primary key must be defined on the HBase rowkey field (rowkey field must be declared). If the PRIMARY KEY clause is not declared, the HBase connector will take rowkey as the primary key by default. For details, see `HBase SQL Connector <https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/connectors/table/hbase/>`__.

-  :ref:`Source Table <dli_08_15043>`
-  :ref:`HBase Result Table <dli_08_15044>`
-  :ref:`Dimension Table <dli_08_15045>`

.. toctree::
   :maxdepth: 1
   :hidden: 

   source_table
   hbase_result_table
   dimension_table
