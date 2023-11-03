:original_name: dli_08_0303.html

.. _dli_08_0303:

JDBC Source Table
=================

Function
--------

The JDBC connector is a Flink's built-in connector to read data from a database.

Prerequisites
-------------

-  An enhanced datasource connection with the database has been established, so that you can configure security group rules as required.

Syntax
------

.. code-block::

   create table jbdcSource (
     attr_name attr_type
     (',' attr_name attr_type)*
     (','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
     (',' watermark for rowtime_column_name as watermark-strategy_expression)
   )
   with (
     'connector.type' = 'jdbc',
     'connector.url' = '',
     'connector.table' = '',
     'connector.username' = '',
     'connector.password' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +--------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                            | Mandatory             | Description                                                                                                                    |
   +======================================+=======================+================================================================================================================================+
   | connector.type                       | Yes                   | Data source type. Set this parameter to **jdbc**.                                                                              |
   +--------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | connector.url                        | Yes                   | Database URL                                                                                                                   |
   +--------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | connector.table                      | Yes                   | Name of the table where the data to be read from the database is located                                                       |
   +--------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | connector.driver                     | No                    | Driver required for connecting to the database If you do not set this parameter, the automatically extracted URL will be used. |
   +--------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | connector.username                   | No                    | Database authentication username. This parameter must be configured in pair with **connector.password**.                       |
   +--------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | connector.password                   | No                    | Database authentication password. This parameter must be configured in pair with **connector.username**.                       |
   +--------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | connector.read.partition.column      | No                    | Name of the column used to partition the input                                                                                 |
   |                                      |                       |                                                                                                                                |
   |                                      |                       | This parameter is mandatory if **connector.read.partition.lower-bound**, **connector.read.partition.upper-bound**, and         |
   |                                      |                       |                                                                                                                                |
   |                                      |                       | **connector.read.partition.num** are configured.                                                                               |
   +--------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | connector.read.partition.lower-bound | No                    | Lower bound of values to be fetched for the first partition                                                                    |
   |                                      |                       |                                                                                                                                |
   |                                      |                       | This parameter is mandatory if **connector.read.partition.column**, **connector.read.partition.upper-bound**, and              |
   |                                      |                       |                                                                                                                                |
   |                                      |                       | **connector.read.partition.num** are configured.                                                                               |
   +--------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | connector.read.partition.upper-bound | No                    | Upper bound of values to be fetched for the last partition                                                                     |
   |                                      |                       |                                                                                                                                |
   |                                      |                       | This parameter is mandatory if **connector.read.partition.column**, **connector.read.partition.lower-bound**, and              |
   |                                      |                       |                                                                                                                                |
   |                                      |                       | **connector.read.partition.num** are configured.                                                                               |
   +--------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | connector.read.partition.num         | No                    | Number of partitions to be created                                                                                             |
   |                                      |                       |                                                                                                                                |
   |                                      |                       | This parameter is mandatory if **connector.read.partition.column**, **connector.read.partition.upper-bound**, and              |
   |                                      |                       |                                                                                                                                |
   |                                      |                       | **connector.read.partition.upper-bound** are configured.                                                                       |
   +--------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | connector.read.fetch-size            | No                    | Number of rows fetched from the database each time The default value is **0**, indicating the hint is ignored.                 |
   +--------------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

None

Example
-------

.. code-block::

   create table jdbcSource (
     car_id STRING,
     car_owner STRING,
     car_age INT,
     average_speed INT,
     total_miles INT)
   with (
     'connector.type' = 'jdbc',
     'connector.url' = 'jdbc:mysql://xx.xx.xx.xx:3306/xx',
     'connector.table' = 'jdbc_table_name',
     'connector.driver' = 'com.mysql.jdbc.Driver',
     'connector.username' = 'xxx',
     'connector.password' = 'xxxxxx'
   );
