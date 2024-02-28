:original_name: dli_08_0112.html

.. _dli_08_0112:

Type Conversion Functions
=========================

Syntax
------

.. code-block::

   CAST(value AS type)

Syntax Description
------------------

This function is used to forcibly convert types.

Precautions
-----------

-  If the input is **NULL**, **NULL** is returned.
-  Flink jobs do not support the conversion of **bigint** to **timestamp** using CAST. You can convert it using **to_timestamp** or **to_localtimestamp**.

Example
-------

Convert amount into a string. The specified length of the string is invalid after the conversion.

.. code-block::

   insert into temp select cast(amount as VARCHAR(10)) from source_stream;

Common Type Conversion Functions
--------------------------------

.. table:: **Table 1** Common type conversion functions

   +--------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------+
   | Function                                                     | Description                                                                                                          |
   +==============================================================+======================================================================================================================+
   | :ref:`cast(v1 as varchar) <dli_08_0112__li164511853108>`     | Converts **v1** to a string. The value of **v1** can be of the numeric type or of the timestamp, date, or time type. |
   +--------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------+
   | :ref:`cast (v1 as int) <dli_08_0112__li191092001410>`        | Converts **v1** to the **int** type. The value of **v1** can be a number or a character.                             |
   +--------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------+
   | :ref:`cast(v1 as timestamp) <dli_08_0112__li14645122173716>` | Converts **v1** to the **timestamp** type. The value of **v1** can be of the **string**, **date**, or **time** type. |
   +--------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------+
   | :ref:`cast(v1 as date) <dli_08_0112__li18269229477>`         | Converts **v1** to the **date** type. The value of **v1** can be of the **string** or **timestamp** type.            |
   +--------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------+

-  .. _dli_08_0112__li164511853108:

   cast(v1 as varchar)

   -  Test statement

      .. code-block::

         SELECT cast(content as varchar) FROM T1;

   -  Test data and result

      .. table:: **Table 2** T1

         ============= =======
         content (INT) varchar
         ============= =======
         5             "5"
         ============= =======

-  .. _dli_08_0112__li191092001410:

   cast (v1 as int)

   -  Test statement

      .. code-block::

         SELECT cast(content as int) FROM T1;

   -  Test data and result

      .. table:: **Table 3** T1

         ================ ===
         content (STRING) int
         ================ ===
         "5"              5
         ================ ===

-  .. _dli_08_0112__li14645122173716:

   cast(v1 as timestamp)

   -  Test statement

      .. code-block::

         SELECT cast(content as timestamp) FROM T1;

   -  Test data and result

      .. table:: **Table 4** T1

         ===================== =============
         content (STRING)      timestamp
         ===================== =============
         "2018-01-01 00:00:01" 1514736001000
         ===================== =============

-  .. _dli_08_0112__li18269229477:

   cast(v1 as date)

   -  Test statement

      .. code-block::

         SELECT cast(content as date) FROM T1;

   -  Test data and result

      .. table:: **Table 5** T1

         =================== ============
         content (TIMESTAMP) date
         =================== ============
         1514736001000       "2018-01-01"
         =================== ============

Detailed Sample Code
--------------------

::

   /** source **/
   CREATE
   SOURCE STREAM car_infos (cast_int_to_varchar int, cast_String_to_int string,
   case_string_to_timestamp string, case_timestamp_to_date timestamp) WITH (
     type = "dis",
     region = "xxxxx",
     channel = "dis-input",
     partition_count = "1",
     encode = "json",
     offset = "13",
     json_config =
   "cast_int_to_varchar=cast_int_to_varchar;cast_String_to_int=cast_String_to_int;case_string_to_timestamp=case_string_to_timestamp;case_timestamp_to_date=case_timestamp_to_date"

   );
   /** sink **/
   CREATE
   SINK STREAM cars_infos_out (cast_int_to_varchar varchar, cast_String_to_int
   int, case_string_to_timestamp timestamp, case_timestamp_to_date date) WITH (
     type = "dis",
     region = "xxxxx",
     channel = "dis-output",
     partition_count = "1",
     encode = "json",
     offset = "4",
     json_config =
   "cast_int_to_varchar=cast_int_to_varchar;cast_String_to_int=cast_String_to_int;case_string_to_timestamp=case_string_to_timestamp;case_timestamp_to_date=case_timestamp_to_date",
     enable_output_null="true"
   );
   /** Statistics on static car information**/
   INSERT
   INTO
     cars_infos_out
   SELECT
     cast(cast_int_to_varchar as varchar),
     cast(cast_String_to_int as int),
     cast(case_string_to_timestamp as timestamp),
     cast(case_timestamp_to_date as date)
   FROM
     car_infos;

Returned data

.. code-block::

   {"case_string_to_timestamp":1514736001000,"cast_int_to_varchar":"5","case_timestamp_to_date":"2018-01-01","cast_String_to_int":100}
