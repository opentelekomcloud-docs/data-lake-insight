:original_name: dli_08_0345.html

.. _dli_08_0345:

Print Result Table
==================

Function
--------

The print connector exports your data output to the **error** file or the **out** file of TaskManager. It is mainly used for code debugging and output viewing.

Syntax
------

::

   create table printSink (
     attr_name attr_type (',' attr_name attr_type) * (',' PRIMARY KEY (attr_name,...) NOT ENFORCED)
   ) with (
     'connector' = 'print',
     'print-identifier' = '',
     'standard-error' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +-----------------------+-----------------------+--------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                          |
   +=======================+=======================+======================================================================================+
   | connector             | Yes                   | The value is fixed to **print**.                                                     |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------+
   | print-identifier      | No                    | Message that identifies print and is prefixed to the output of the value.            |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------+
   | standard-error        | No                    | The value can be only **true** or **false**. The default value is **false**.         |
   |                       |                       |                                                                                      |
   |                       |                       | -  If the value is **true**, data is output to the error file of the TaskManager.    |
   |                       |                       | -  If the value is **false**, data is output to the **out** file of the TaskManager. |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------+

Example
-------

Read data from Kafka and export the data to the **out** file of TaskManager. You can view the output in the exported file.

.. code-block::

   create table kafkaSource(
     attr0 string,
     attr1 boolean,
     attr3 decimal(38, 18),
     attr4 TINYINT,
     attr5 smallint,
     attr6 int,
     attr7 bigint,
     attr8 float,
     attr9 double,
     attr10 date,
     attr11 time,
     attr12 timestamp(3)
   ) with (
     'connector.type' = 'kafka',
     'connector.version' = '0.11',
     'connector.topic' = 'test_json',
     'connector.properties.bootstrap.servers' = 'xx.xx.xx.xx:9092',
     'connector.properties.group.id' = 'test_print',
     'connector.startup-mode' = 'latest-offset',
     'format.type' = 'csv'
   );

   create table printTable(
     attr0 string,
     attr1 boolean,
     attr3 decimal(38,18),
     attr4 TINYINT,
     attr5 smallint,
     attr6 int,
     attr7 bigint,
     attr8 float,
     attr9 double,
     attr10 date,
     attr11 time,
     attr12 timestamp(3),
     attr13 array<string>,
     attr14 row<attr15 float, attr16 timestamp(3)>,
     attr17 map<int, bigint>
   ) with (
     "connector" = "print"
   );

   insert into
     printTable
   select
     attr0,
     attr1,
     attr3,
     attr4,
     attr5,
     attr6,
     attr7,
     attr8,
     attr9,
     attr10,
     attr11,
     attr12,
     array [cast(attr0 as string), cast(attr0 as string)],
     row(
       cast(attr8 as float),
       cast(attr12 as timestamp(3))
     ),
     map [cast(attr6 as int), cast(attr7 as bigint)]
   from
     kafkaSource;
