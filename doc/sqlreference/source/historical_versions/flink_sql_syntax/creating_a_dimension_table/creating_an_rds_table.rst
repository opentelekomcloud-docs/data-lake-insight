:original_name: dli_08_0261.html

.. _dli_08_0261:

Creating an RDS Table
=====================

Create an RDS/DWS table to connect to the source stream.

For details about the JOIN syntax, see :ref:`JOIN <dli_08_0106>`.

Prerequisites
-------------

-  Ensure that you have created a PostgreSQL or MySQL RDS instance in RDS.

   For details about how to create an RDS instance, see **Creating an Instance** in the *Relational Database Service User Guide*.

-  In this scenario, jobs must run on the dedicated queue of DLI. Therefore, DLI must interconnect with the enhanced datasource connection that has been connected with RDS instance. You can also set the security group rules as required.

   For details about how to create an enhanced datasource connection, see **Enhanced Datasource Connections** in the *Data Lake Insight User Guide*.

   For details about how to configure security group rules, see **Security Group** in the *Virtual Private Cloud User Guide*.

Syntax
------

::

   CREATE TABLE  table_id (
     car_id STRING,
     car_owner STRING,
     car_brand STRING,
     car_price INT
   )
     WITH (
       type = "rds",
       username = "",
       password = "",
       db_url = "",
       table_name = ""
     );

Keywords
--------

.. table:: **Table 1** Keywords

   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                                                                                                                                                               |
   +=======================+=======================+===========================================================================================================================================================================================================================================================================================================+
   | type                  | Yes                   | Output channel type. Value **rds** indicates that data is stored to RDS.                                                                                                                                                                                                                                  |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | username              | Yes                   | Username for connecting to a database.                                                                                                                                                                                                                                                                    |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password              | Yes                   | Password for connecting to a database.                                                                                                                                                                                                                                                                    |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | db_url                | Yes                   | Database connection address, for example, **{database_type}://ip:port/database**.                                                                                                                                                                                                                         |
   |                       |                       |                                                                                                                                                                                                                                                                                                           |
   |                       |                       | Currently, two types of database connections are supported: MySQL and PostgreSQL.                                                                                                                                                                                                                         |
   |                       |                       |                                                                                                                                                                                                                                                                                                           |
   |                       |                       | -  MySQL: 'mysql://ip:port/database'                                                                                                                                                                                                                                                                      |
   |                       |                       | -  PostgreSQL: 'postgresql://ip:port/database'                                                                                                                                                                                                                                                            |
   |                       |                       |                                                                                                                                                                                                                                                                                                           |
   |                       |                       |    .. note::                                                                                                                                                                                                                                                                                              |
   |                       |                       |                                                                                                                                                                                                                                                                                                           |
   |                       |                       |       To create a DWS dimension table, set the database connection address to a DWS database address. If the DWS database version is later than 8.1.0, the open-source PostgreSQL driver cannot be used for connection. You need to use the GaussDB driver for connection.                                |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name            | Yes                   | Indicates the name of the database table for data query.                                                                                                                                                                                                                                                  |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | db_columns            | No                    | Indicates the mapping of stream attribute fields between the sink stream and database table. This parameter is mandatory when the stream attribute fields in the sink stream do not match those in the database table. The parameter value is in the format of dbtable_attr1,dbtable_attr2,dbtable_attr3. |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cache_max_num         | No                    | Indicates the maximum number of cached query results. The default value is **32768**.                                                                                                                                                                                                                     |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cache_time            | No                    | Indicates the maximum duration for caching database query results in the memory. The unit is millisecond. The default value is **10000**. The value **0** indicates that caching is disabled.                                                                                                             |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

The RDS table is used to connect to the source stream.

::

   CREATE SOURCE STREAM car_infos (
     car_id STRING,
     car_owner STRING,
     car_brand STRING,
     car_price INT
   )
     WITH (
       type = "dis",
       region = "",
       channel = "dliinput",
       encode = "csv",
       field_delimiter = ","
     );

   CREATE TABLE  db_info (
     car_id STRING,
     car_owner STRING,
     car_brand STRING,
     car_price INT
   )
     WITH (
       type = "rds",
       username = "root",
       password = "******",
       db_url = "postgresql://192.168.0.0:2000/test1",
       table_name = "car"
   );

   CREATE SINK STREAM audi_cheaper_than_30w (
     car_id STRING,
     car_owner STRING,
     car_brand STRING,
     car_price INT
   )
     WITH (
       type = "dis",
       region = "",
       channel = "dlioutput",
       partition_key = "car_owner",
       encode = "csv",
       field_delimiter = ","
     );

   INSERT INTO audi_cheaper_than_30w
   SELECT a.car_id, b.car_owner, b.car_brand, b.car_price
   FROM car_infos as a join db_info as b on a.car_id = b.car_id;

.. note::

   To create a DWS dimension table, set the database connection address to a DWS database address. If the DWS database version is later than 8.1.0, the open-source PostgreSQL driver cannot be used for connection. You need to use the GaussDB driver for connection.
