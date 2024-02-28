:original_name: dli_08_0122.html

.. _dli_08_0122:

Creating a DLI Table and Associating It with OpenTSDB
=====================================================

Function
--------

Run the CREATE TABLE statement to create the DLI table and associate it with the existing metric in OpenTSDB. This syntax supports the OpenTSDB of CloudTable and MRS.

Prerequisites
-------------

Before creating a DLI table and associating it with OpenTSDB, you need to create a datasource connection. For details about operations on the management console, see

Syntax
------

::

   CREATE TABLE [IF NOT EXISTS] UQUERY_OPENTSDB_TABLE_NAME
     USING OPENTSDB OPTIONS (
     'host' = 'xx;xx',
     'metric' = 'METRIC_NAME',
     'tags' = 'TAG1,TAG2');

Keywords
--------

.. table:: **Table 1** CREATE TABLE keywords

   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                                                                                                                        |
   +===================================+====================================================================================================================================================================================================================================================================================+
   | host                              | OpenTSDB IP address.                                                                                                                                                                                                                                                               |
   |                                   |                                                                                                                                                                                                                                                                                    |
   |                                   | Before obtaining the OpenTSDB IP address, you need to create a datasource connection first..                                                                                                                                                                                       |
   |                                   |                                                                                                                                                                                                                                                                                    |
   |                                   | -  After successfully created a connection, you can access the CloudTable OpenTSDB by entering the IP address of the OpenTSDB.                                                                                                                                                     |
   |                                   | -  You can also access the MRS OpenTSDB. If you have created an enhanced datasource connection, enter the IP address and port number of the node where the OpenTSDB is located. The format is **IP:PORT**. If the OpenTSDB has multiple nodes, enter one of the node IP addresses. |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | metric                            | Name of the metric in OpenTSDB corresponding to the DLI table to be created.                                                                                                                                                                                                       |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tags                              | Tags corresponding to the metric. The tags are used for classification, filtering, and quick retrieval. You can set 1 to 8 tags, which are separated by commas (,). The parameter value includes values of all tagKs in the corresponding metric.                                  |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

When creating a DLI table, you do not need to specify the **timestamp** and **value** fields. The system automatically builds the following fields based on the specified tags. The fields **TAG1** and **TAG2** are specified by tags.

-  TAG1 String
-  TAG2 String
-  timestamp Timestamp
-  value double

Example
-------

::

   CREATE table opentsdb_table
     USING OPENTSDB OPTIONS (
     'host' = 'opentsdb-3xcl8dir15m58z3.cloudtable.com:4242',
     'metric' = 'city.temp',
     'tags' = 'city,location');
