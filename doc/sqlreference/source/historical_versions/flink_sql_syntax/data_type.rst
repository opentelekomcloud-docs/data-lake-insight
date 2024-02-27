:original_name: dli_08_0207.html

.. _dli_08_0207:

Data Type
=========

Overview
--------

Data type is a basic attribute of data and used to distinguish different types of data. Different data types occupy different storage space and support different operations. Data is stored in data tables in the database. Each column of a data table defines the data type. During storage, data must be stored according to data types.

Similar to the open source community, Flink SQL of the big data platform supports both native data types and complex data types.

Primitive Data Types
--------------------

:ref:`Table 1 <dli_08_0207__t48a8d4da90014446bdf59256969b49c0>` lists native data types supported by Flink SQL.

.. _dli_08_0207__t48a8d4da90014446bdf59256969b49c0:

.. table:: **Table 1** Primitive data types

   +---------------------------------+----------------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------+
   | Data Type                       | Description                                                    | Storage Space   | Value Range                                                                                 |
   +=================================+================================================================+=================+=============================================================================================+
   | VARCHAR                         | Character with a variable length                               | ``-``           | ``-``                                                                                       |
   +---------------------------------+----------------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------+
   | BOOLEAN                         | Boolean                                                        | ``-``           | TRUE/FALSE                                                                                  |
   +---------------------------------+----------------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------+
   | TINYINT                         | Signed integer                                                 | 1 byte          | -128-127                                                                                    |
   +---------------------------------+----------------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------+
   | SMALLINT                        | Signed integer                                                 | 2 bytes         | -32768-32767                                                                                |
   +---------------------------------+----------------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------+
   | INT                             | Signed integer                                                 | 4 bytes         | -2147483648 to 2147483647                                                                   |
   +---------------------------------+----------------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------+
   | INTEGER                         | Signed integer                                                 | 4 bytes         | -2147483648 to 2147483647                                                                   |
   +---------------------------------+----------------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------+
   | BIGINT                          | Signed integer                                                 | 8 bytes         | -9223372036854775808 to 9223372036854775807                                                 |
   +---------------------------------+----------------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------+
   | REAL                            | Single-precision floating point                                | 4 bytes         | ``-``                                                                                       |
   +---------------------------------+----------------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------+
   | FLOAT                           | Single-precision floating point                                | 4 bytes         | ``-``                                                                                       |
   +---------------------------------+----------------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------+
   | DOUBLE                          | Double-precision floating-point                                | 8 bytes         | ``-``                                                                                       |
   +---------------------------------+----------------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------+
   | DECIMAL                         | Data type of valid fixed places and decimal places             | ``-``           | ``-``                                                                                       |
   +---------------------------------+----------------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------+
   | DATE                            | Date type in the format of yyyy-MM-dd, for example, 2014-05-29 | ``-``           | **DATE** does not contain time information. Its value ranges from 0000-01-01 to 9999-12-31. |
   +---------------------------------+----------------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------+
   | TIME                            | Time type in the format of HH:MM:SS                            | ``-``           | ``-``                                                                                       |
   |                                 |                                                                |                 |                                                                                             |
   |                                 | For example, 20:17:40                                          |                 |                                                                                             |
   +---------------------------------+----------------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------+
   | TIMESTAMP(3)                    | Timestamp of date and time                                     | ``-``           | ``-``                                                                                       |
   |                                 |                                                                |                 |                                                                                             |
   |                                 | For example, 1969-07-20 20:17:40                               |                 |                                                                                             |
   +---------------------------------+----------------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------+
   | INTERVAL timeUnit [TO timeUnit] | Time interval                                                  | ``-``           | ``-``                                                                                       |
   |                                 |                                                                |                 |                                                                                             |
   |                                 | For example, INTERVAL '1:5' YEAR TO MONTH, INTERVAL '45' DAY   |                 |                                                                                             |
   +---------------------------------+----------------------------------------------------------------+-----------------+---------------------------------------------------------------------------------------------+

Complex Data Types
------------------

Flink SQL supports complex data types and complex type nesting. :ref:`Table 2 <dli_08_0207__t1f67ad6ffe1b4d7c8a38b2a156833dad>` describes complex data types.

.. _dli_08_0207__t1f67ad6ffe1b4d7c8a38b2a156833dad:

.. table:: **Table 2** Complex data types

   +-----------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------+-------------------------------------------------------------------------------------+----------------------------------------------------------+
   | Data Type | Description                                                                                                                                                                                                                    | Declaration Method      | Reference Method                                                                    | Construction Method                                      |
   +===========+================================================================================================================================================================================================================================+=========================+=====================================================================================+==========================================================+
   | ARRAY     | Indicates a group of ordered fields that are of the same data type.                                                                                                                                                            | ARRAY[TYPE]             | Variable name **[subscript]**. The subscript starts from 1, for example, **v1[1]**. | Array[value1, value2, ...] as v1                         |
   +-----------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------+-------------------------------------------------------------------------------------+----------------------------------------------------------+
   | MAP       | Indicates a group of unordered key/value pairs. The key must be native data type, but the value can be either native data type or complex data type. The type of the same MAP key, as well as the MAP value, must be the same. | **MAP [TYPE, TYPE]**    | Variable name **[key]**, for example, **v1[key]**                                   | Map[key, value, key2, value2, key3, value3.......] as v1 |
   +-----------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------+-------------------------------------------------------------------------------------+----------------------------------------------------------+
   | ROW       | Indicates a group of named fields. The data types of the fields can be different.                                                                                                                                              | ROW<a1 TYPE1, a2 TYPE2> | Variable name. Field name, for example, **v1.a1**.                                  | Row('1',2) as v1                                         |
   +-----------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------+-------------------------------------------------------------------------------------+----------------------------------------------------------+

Here is a sample code:

::

   CREATE SOURCE STREAM car_infos (
     car_id STRING,
     address ROW<city STRING, province STRING, country STRING>,
     average_speed MAP[STRING, LONG],
     speeds ARRAY[LONG]
   )
     WITH (
       type = "dis",
       region = "xxx",
       channel = "dliinput",
       encode = "json"
   );

   CREATE temp STREAM car_speed_infos (
     car_id STRING,
     province STRING,
     average_speed LONG,
     start_speed LONG
   );

   INSERT INTO car_speed_infos SELECT
      car_id,
      address.province,
      average_speed[address.city],
      speeds[1]
   FROM car_infos;

Complex Type Nesting
--------------------

-  .. _dli_08_0207__li0801848164311:

   JSON format enhancement

   The following uses Source as an example. The method of using Sink is the same.

   -  **json_schema** can be configured.

      After **json_schema** is configured, fields in DDL can be automatically generated from **json_schema** without declaration. Here is a sample code:

      .. code-block::

         CREATE SOURCE STREAM data_with_schema WITH (
                type = "dis",
                region = "xxx",
                channel = "dis-in",
                encode = "json",
                json_schema = '{"definitions":{"address":{"type":"object","properties":{"street_address":{"type":"string"},"city":{"type":"string"},"state":{"type":"string"}},"required":["street_address","city","state"]}},"type":"object","properties":{"billing_address":{"$ref":"#/definitions/address"},"shipping_address":{"$ref":"#/definitions/address"},"optional_address":{"oneOf":[{"type":"null"},{"$ref":"#/definitions/address"}]}}}'
              );

              CREATE SINK STREAM buy_infos (
                billing_address_city STRING,
                shipping_address_state string
              ) WITH (
                type = "obs",
                encode = "csv",
                region = "xxx" ,
                field_delimiter = ",",
                row_delimiter = "\n",
                obs_dir = "bucket/car_infos",
                file_prefix = "over",
                rolling_size = "100m"
              );

              insert into buy_infos select billing_address.city, shipping_address.state from data_with_schema;

      Example data

      .. code-block::

         {
          "billing_address":
           {
            "street_address":"xxx",
            "city":"xxx",
            "state":"xxx"
            },
          "shipping_address":
           {
            "street_address":"xxx",
            "city":"xxx",
            "state":"xxx"
           }
         }

   -  The **json_schema** and **json_config** parameters can be left empty. For details about how to use **json_config**, see the example in :ref:`Open-Source Kafka Source Stream <dli_08_0239>`.

      In this case, the attribute name in the DDL is used as the JSON key for parsing by default.

      The following is example data. It contains nested JSON fields, such as **billing_address** and **shipping_address**, and non-nested fields **id** and **type2**.

      .. code-block::

         {
          "id":"1",
          "type2":"online",
          "billing_address":
           {
            "street_address":"xxx",
            "city":"xxx",
            "state":"xxx"
            },
          "shipping_address":
           {
            "street_address":"xxx",
            "city":"xxx",
            "state":"xxx"
           }
         }

      The table creation and usage examples are as follows:

      .. code-block::

         CREATE SOURCE STREAM car_info_data (
                id STRING,
                type2 STRING,
                billing_address Row<street_address string, city string, state string>,
                shipping_address Row<street_address string, city string, state string>,
                optional_address Row<street_address string, city string, state string>
              ) WITH (
                type = "dis",
                region = "xxx",
                channel = "dis-in",
                encode = "json"
              );

             CREATE SINK STREAM buy_infos (
                id STRING,
                type2 STRING,
                billing_address_city STRING,
                shipping_address_state string
              ) WITH (
                type = "obs",
                encode = "csv",
                region = "xxx",
                field_delimiter = ",",
                row_delimiter = "\n",
                obs_dir = "bucket/car_infos",
                file_prefix = "over",
                rolling_size = "100m"
              );

              insert into buy_infos select id, type2, billing_address.city, shipping_address.state from car_info_data;

-  Complex data types supported by sink serialization

   -  Currently, only the CSV and JSON formats support complex data types.

   -  For details about the JSON format, see :ref:`Json format enhancement <dli_08_0207__li0801848164311>`.

   -  There is no standard format for CSV files. Therefore, only sink parsing is supported.

   -  Output format: It is recommended that the output format be the same as that of the native Flink.

      Map: {key1=Value1, key2=Value2}

      Row: Attributes are separated by commas (,), for example, **Row(1,'2') => 1,'2'**.
