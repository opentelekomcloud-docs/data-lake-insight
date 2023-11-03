:original_name: dli_08_0311.html

.. _dli_08_0311:

JDBC Result Table
=================

Function
--------

DLI exports the output data of the Flink job to RDS.

Prerequisites
-------------

-  An enhanced datasource connection with the database has been established, so that you can configure security group rules as required.

Syntax
------

::

   create table jdbcSink (
     attr_name attr_type
     (',' attr_name attr_type)*
     (','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
   )
   with (
     'connector.type' = 'jdbc',
     'connector.url' = '',
     'connector.table' = '',
     'connector.driver' = '',
     'connector.username' = '',
     'connector.password' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                              | Mandatory | Description                                                                                                                                                                                                               |
   +========================================+===========+===========================================================================================================================================================================================================================+
   | connector.type                         | Yes       | Data source type. Set this parameter to **jdbc**.                                                                                                                                                                         |
   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.url                          | Yes       | Database URL                                                                                                                                                                                                              |
   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.table                        | Yes       | Name of the table where the data to be read from the database is located                                                                                                                                                  |
   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.driver                       | No        | Driver required for connecting to the database If you do not set this parameter, the automatically extracted URL will be used.                                                                                            |
   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.username                     | No        | Username for accessing the database                                                                                                                                                                                       |
   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.password                     | No        | Password for accessing the database                                                                                                                                                                                       |
   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.write.flush.max-rows         | No        | Maximum number of rows to be updated when data is written. The default value is **5000**.                                                                                                                                 |
   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.write.flush.interval         | No        | Interval for data update. The unit can be ms, milli, millisecond/s, sec, second/min or minute. If this parameter is not set, the value is not updated based on the interval by default.                                   |
   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.write.max-retries            | No        | Maximum number of attempts to write data if failed. The default value is **3**.                                                                                                                                           |
   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.write.exclude-update-columns | No        | Columns excluded for data update. The default value is empty, indicating that when data with the same primary key is updated, the update of the specified field is ignored. The primary key column is ignored by default. |
   +----------------------------------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

None

Example
-------

Output data from stream **jdbcSink** to the MySQL database.

::

   create table jdbcSink(
     car_id STRING,
     car_owner STRING,
     car_brand STRING,
     car_speed INT
   )
   with (
     'connector.type' = 'jdbc',
     'connector.url' = 'jdbc:mysql://xx.xx.xx.xx:3306/xx',
     'connector.table' = 'jdbc_table_name',
     'connector.driver' = 'com.mysql.jdbc.Driver',
     'connector.username' = 'xxx',
     'connector.password' = 'xxxxxx'
   );
