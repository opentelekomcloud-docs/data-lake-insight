:original_name: dli_08_0230.html

.. _dli_08_0230:

Creating a DLI Table and Associating It with DDS
================================================

Function
--------

This statement is used to create a DLI table and associate it with an existing DDS collection.

.. note::

   In Spark cross-source development scenarios, there is a risk of password leakage if datasource authentication information is directly configured. You are advised to use the datasource authentication provided by DLI.

Prerequisites
-------------

Before creating a DLI table and associating it with DDS, you need to create a datasource connection and bind it to a queue. For details about operations on the management console, see

Syntax
------

::

   CREATE TABLE [IF NOT EXISTS] TABLE_NAME(
       FIELDNAME1 FIELDTYPE1,
       FIELDNAME2 FIELDTYPE2)
     USING MONGO OPTIONS (
     'url'='IP:PORT[,IP:PORT]/[DATABASE][.COLLECTION][AUTH_PROPERTIES]',
     'database'='xx',
     'collection'='xx',
     'passwdauth' = 'xxx',
     'encryption' = 'true'
   );

.. note::

   Document Database Service (DDS) is fully compatible with the MongoDB protocol. Therefore, the syntax used is **using mongo options**.

Keywords
--------

.. table:: **Table 1** CREATE TABLE keywords

   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                              |
   +===================================+==========================================================================================================================================================================+
   | url                               | Before obtaining the DDS IP address, you need to create a datasource connection first..                                                                                  |
   |                                   |                                                                                                                                                                          |
   |                                   | After creating an enhanced datasource connection, use the random connection address provided by DDS. The format is as follows:                                           |
   |                                   |                                                                                                                                                                          |
   |                                   | "IP:PORT[,IP:PORT]/[DATABASE][.COLLECTION][AUTH_PROPERTIES]"                                                                                                             |
   |                                   |                                                                                                                                                                          |
   |                                   | Example: "192.168.4.62:8635,192.168.5.134:8635/test?authSource=admin"                                                                                                    |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | database                          | DDS database name. If the database name is specified in the URL, the database name in the URL does not take effect.                                                      |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | collection                        | Collection name in the DDS. If the collection is specified in the URL, the collection in the URL does not take effect.                                                   |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | user                              | (Discarded) Username for accessing the DDS cluster.                                                                                                                      |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password                          | (Discarded) Password for accessing the DDS cluster.                                                                                                                      |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | passwdauth                        | Datasource password authentication name. For details about how to create datasource authentication, see Datasource Authentication in the *Data Lake Insight User Guide*. |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encryption                        | Set this parameter to **true** when datasource password authentication is used.                                                                                          |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. note::

   If a collection already exists in DDS, you do not need to specify schema information when creating a table. DLI automatically generates schema information based on data in the collection.

Example
-------

::

   create table 1_datasource.test_table1(id string, name string, age int) using mongo options(
     'url' = '192.168.4.62:8635,192.168.5.134:8635/test?authSource=admin',
     'database' = 'test',
     'collection' = 'test',
     'passwdauth' = 'xxx',
     'encryption' = 'true');
