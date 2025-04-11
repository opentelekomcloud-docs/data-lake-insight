:original_name: dli_08_0344.html

.. _dli_08_0344:

ClickHouse Result Table
=======================

Function
--------

DLI exports Flink job data to ClickHouse result tables.

ClickHouse is a column-based database oriented to online analysis and processing. It supports SQL query and provides good query performance. The aggregation analysis and query performance based on large and wide tables is excellent, which is one order of magnitude faster than other analytical databases.

Prerequisites
-------------

You have established an enhanced datasource connection to ClickHouse and set the port in the security group rule of the ClickHouse cluster as needed.

For details about how to set up an enhanced datasource connection. For details, see "Enhanced Datasource Connection" in the *Data Lake Insight User Guide*.

Precautions
-----------

-  When you create a ClickHouse cluster for MRS, set the cluster version to MRS 3.1.0 and do not enable Kerberos authentication.

-  Do not define a primary key in Flink SQL statements. Do not use any syntax that generates primary keys, such as **insert into clickhouseSink select id, cout(*) from sourceName group by id**.

-  Flink supports the following data types: string, tinyint, smallint, int, long, float, double, date, timestamp, decimal, and Array.

   The array supports only the int, bigint, string, float, and double data types.

Syntax
------

::

   create table clickhouseSink (
     attr_name attr_type
     (',' attr_name attr_type)*
   )
   with (
     'connector.type' = 'clickhouse',
     'connector.url' = '',
     'connector.table' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +--------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                      | Mandatory             | Description                                                                                                                                                                                                                                                                                                                                 |
   +================================+=======================+=============================================================================================================================================================================================================================================================================================================================================+
   | connector.type                 | Yes                   | Result table type. Set this parameter to **clickhouse**.                                                                                                                                                                                                                                                                                    |
   +--------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.url                  | Yes                   | ClickHouse URL.                                                                                                                                                                                                                                                                                                                             |
   |                                |                       |                                                                                                                                                                                                                                                                                                                                             |
   |                                |                       | Parameter format: **jdbc:clickhouse://**\ *ClickHouseBalancer instance IP address*\ **:**\ *HTTP port number for ClickHouseBalancer instances*\ **/**\ *Database name*                                                                                                                                                                      |
   |                                |                       |                                                                                                                                                                                                                                                                                                                                             |
   |                                |                       | -  IP address of a ClickHouseBalancer instance:                                                                                                                                                                                                                                                                                             |
   |                                |                       |                                                                                                                                                                                                                                                                                                                                             |
   |                                |                       |    Log in to the MRS management console, click a cluster name, and choose **Components** > **ClickHouse** > **Instance** to obtain the service IP address of the ClickHouseBalancer instance.                                                                                                                                               |
   |                                |                       |                                                                                                                                                                                                                                                                                                                                             |
   |                                |                       | -  HTTP port of a ClickHouseBalancer instance:                                                                                                                                                                                                                                                                                              |
   |                                |                       |                                                                                                                                                                                                                                                                                                                                             |
   |                                |                       |    Log in to the MRS management console, click the target cluster name. On the displayed page, choose **Components** > **ClickHouse**. In the **Service Configuration** tab, choose **ClickHouseBalancer** from the **All Roles** dropdown list and search for **lb_http_port** to configure the parameter. The default value is **21425**. |
   |                                |                       |                                                                                                                                                                                                                                                                                                                                             |
   |                                |                       | -  The database name is the name of the database created for the ClickHouse cluster.                                                                                                                                                                                                                                                        |
   +--------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.table                | Yes                   | Name of the ClickHouse table to be created                                                                                                                                                                                                                                                                                                  |
   +--------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.driver               | No                    | Driver required for connecting to the database                                                                                                                                                                                                                                                                                              |
   |                                |                       |                                                                                                                                                                                                                                                                                                                                             |
   |                                |                       | -  If this parameter is not specified during table creation, the driver automatically extracts the value from the ClickHouse URL.                                                                                                                                                                                                           |
   |                                |                       | -  If this parameter is specified during table creation, the value must be **ru.yandex.clickhouse.ClickHouseDriver**.                                                                                                                                                                                                                       |
   +--------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.username             | No                    | Account for connecting the ClickHouse database                                                                                                                                                                                                                                                                                              |
   +--------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.password             | No                    | Password for accessing the ClickHouse database                                                                                                                                                                                                                                                                                              |
   +--------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.write.flush.max-rows | No                    | Maximum number of rows to be updated when data is written. The default value is **5000**.                                                                                                                                                                                                                                                   |
   +--------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.write.flush.interval | No                    | Interval for data update. The unit can be ms, milli, millisecond/s, sec, second/min or minute.                                                                                                                                                                                                                                              |
   +--------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.write.max-retries    | No                    | Maximum number of attempts to write data if failed. The default value is **3**.                                                                                                                                                                                                                                                             |
   +--------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

Read data from a DIS table and insert the data into the **test** table of ClickHouse database **flinktest**.

#. Create a DIS source table **disSource**.

   ::

      create table disSource(
        attr0 string,
        attr1 TINYINT,
        attr2 smallint,
        attr3 int,
        attr4 bigint,
        attr5 float,
        attr6 double,
        attr7 String,
        attr8 string,
        attr9 timestamp(3),
        attr10 timestamp(3),
        attr11 date,
        attr12 decimal(38, 18),
        attr13 decimal(38, 18)
      ) with (
        "connector.type" = "dis",
        "connector.region" = "cn-xxxx-x",
        "connector.channel" = "xxxx",
        "format.type" = 'csv'
      );

#. Create ClickHouse result table **clickhouse** and insert the data from the **disSource** table to the result table.

   .. code-block::

      create table clickhouse(
        attr0 string,
        attr1 TINYINT,
        attr2 smallint,
        attr3 int,
        attr4 bigint,
        attr5 float,
        attr6 double,
        attr7 String,
        attr8 string,
        attr9 timestamp(3),
        attr10 timestamp(3),
        attr11 date,
        attr12 decimal(38, 18),
        attr13 decimal(38, 18),
        attr14 array < int >,
        attr15 array < bigint >,
        attr16 array < float >,
        attr17 array < double >,
        attr18 array < varchar >,
        attr19 array < String >
      ) with (
        'connector.type' = 'clickhouse',
        'connector.url' = 'jdbc:clickhouse://xx.xx.xx.xx:xx/flinktest',
        'connector.table' = 'test'
      );

      insert into
        clickhouse
      select
        attr0,
        attr1,
        attr2,
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
        attr13,
        array [attr3, attr3+1],
        array [cast(attr4 as bigint), cast(attr4+1 as bigint)],
        array [cast(attr12 as float), cast(attr12+1 as float)],
        array [cast(attr13 as double), cast(attr13+1 as double)],
        array ['TEST1', 'TEST2'],
        array [attr7, attr7]
      from
        disSource;
