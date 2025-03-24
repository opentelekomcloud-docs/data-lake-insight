:original_name: dli_08_0461.html

.. _dli_08_0461:

Creating a DLI Table and Associating It with Oracle
===================================================

Function
--------

This statement is used to create a DLI table and associate it with an existing Oracle table.

Prerequisites
-------------

-  Before creating a DLI table and associating it with Oracle, you need to create an enhanced datasource connection.

   For details about operations on the management console, see .

Syntax
------

::

   CREATE TABLE [IF NOT EXISTS] TABLE_NAME
     USING ORACLE OPTIONS (
     'url'='xx',
     'driver'='DRIVER_NAME',
     'dbtable'='db_in_oracle.table_in_oracle',
     'user' = 'xxx',
     'password' = 'xxx',
     'resource' = 'obs://rest-authinfo/tools/oracle/driver/ojdbc6.jar'
   );

Keywords
--------

.. table:: **Table 1** CREATE TABLE keywords

   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                                |
   +===================================+============================================================================================================================================================================================+
   | url                               | URL of the Oracle database.                                                                                                                                                                |
   |                                   |                                                                                                                                                                                            |
   |                                   | The URL can be in either of the following format:                                                                                                                                          |
   |                                   |                                                                                                                                                                                            |
   |                                   | -  Format 1: **jdbc:oracle:thin:@host:port:**\ *SID*, in which *SID* is the unique identifier of the Oracle database.                                                                      |
   |                                   | -  Format 2: **jdbc:oracle:thin:@//host:port/**\ *service_name*. This format is recommended by Oracle. For a cluster, the SID of each node may differ, but their service name is the same. |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | driver                            | Oracle driver class name: **oracle.jdbc.driver.OracleDriver**                                                                                                                              |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dbtable                           | Name of the table associated with the Oracle database or *Username*\ **.**\ *Table name*, for example, **public.table_name**.                                                              |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | user                              | Oracle username.                                                                                                                                                                           |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password                          | Oracle password.                                                                                                                                                                           |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resource                          | OBS path of the Oracle driver package.                                                                                                                                                     |
   |                                   |                                                                                                                                                                                            |
   |                                   | Example: **obs://rest-authinfo/tools/oracle/driver/ojdbc6.jar**                                                                                                                            |
   |                                   |                                                                                                                                                                                            |
   |                                   | If the driver JAR file defined in this parameter is updated, you need to restart the queue for the update to take effect.                                                                  |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

Creating an Oracle datasource table

::

   CREATE TABLE IF NOT EXISTS oracleTest
     USING ORACLE OPTIONS (
     'url'='jdbc:oracle:thin:@//192.168.168.40:1521/helowin',
     'driver'='oracle.jdbc.driver.OracleDriver',
     'dbtable'='test.Student',
     'user' = 'test',
     'password' = 'test',
     'resource' = 'obs://rest-authinfo/tools/oracle/driver/ojdbc6.jar'
   );
