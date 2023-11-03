:original_name: dli_08_0336.html

.. _dli_08_0336:

Type Conversion Function
========================

Syntax
------

.. code-block::

   CAST(value AS type)

Syntax Description
------------------

This function is used to forcibly convert types.

Precautions
-----------

If the input is **NULL**, **NULL** is returned.

Example
-------

The following example converts the **amount** value to an integer.

.. code-block::

   insert into temp select cast(amount as INT) from source_stream;

.. table:: **Table 1** Examples of type conversion functions

   +-----------------------+----------------------------------------------------------------------------------------------------------------------+------------------------------------+
   | Example               | Description                                                                                                          | Example                            |
   +=======================+======================================================================================================================+====================================+
   | cast(v1 as string)    | Converts **v1** to a string. The value of **v1** can be of the numeric type or of the timestamp, date, or time type. | Table T1:                          |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    | content (INT)           |     |
   |                       |                                                                                                                      |    | -------------           |     |
   |                       |                                                                                                                      |    | 5                       |     |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | Statement:                         |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    SELECT                          |
   |                       |                                                                                                                      |      cast(content as varchar)      |
   |                       |                                                                                                                      |    FROM                            |
   |                       |                                                                                                                      |      T1;                           |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | Result:                            |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    "5"                             |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------+------------------------------------+
   | cast (v1 as int)      | Converts **v1** to the **int** type. The value of **v1** can be a number or a character.                             | Table T1:                          |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    | content  (STRING)           | |
   |                       |                                                                                                                      |    | -------------               | |
   |                       |                                                                                                                      |    | "5"                         | |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | Statement:                         |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    SELECT                          |
   |                       |                                                                                                                      |      cast(content as int)          |
   |                       |                                                                                                                      |    FROM                            |
   |                       |                                                                                                                      |      T1;                           |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | Result:                            |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    5                               |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------+------------------------------------+
   | cast(v1 as timestamp) | Converts **v1** to the **timestamp** type. The value of **v1** can be of the **string**, **date**, or **time** type. | Table T1:                          |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    | content  (STRING)          |  |
   |                       |                                                                                                                      |    | -------------              |  |
   |                       |                                                                                                                      |    | "2018-01-01 00:00:01"     |   |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | Statement:                         |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    SELECT                          |
   |                       |                                                                                                                      |      cast(content as timestamp)    |
   |                       |                                                                                                                      |    FROM                            |
   |                       |                                                                                                                      |      T1;                           |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | Result:                            |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    1514736001000                   |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------+------------------------------------+
   | cast(v1 as date)      | Converts **v1** to the **date** type. The value of **v1** can be of the **string** or **timestamp** type.            | Table T1:                          |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    | content  (TIMESTAMP)     |    |
   |                       |                                                                                                                      |    | -------------            |    |
   |                       |                                                                                                                      |    | 1514736001000            |    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | Statement:                         |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    SELECT                          |
   |                       |                                                                                                                      |      cast(content as date)         |
   |                       |                                                                                                                      |    FROM                            |
   |                       |                                                                                                                      |      T1;                           |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | Result:                            |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    "2018-01-01"                    |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------+------------------------------------+

.. note::

   Flink jobs do not support the conversion of **bigint** to **timestamp** using CAST. You can convert it using **to_timestamp**.

Detailed Sample Code
--------------------

.. code-block::

   /** source **/
   CREATE
   TABLE car_infos (cast_int_to_string int, cast_String_to_int string,
   case_string_to_timestamp string, case_timestamp_to_date timestamp(3)) WITH (
     'connector.type' = 'dis',
     'connector.region' = 'xxxxx',
     'connector.channel' = 'dis-input',
     'format.type' = 'json'
   );
   /** sink **/
   CREATE
   TABLE cars_infos_out (cast_int_to_string string, cast_String_to_int
   int, case_string_to_timestamp timestamp(3), case_timestamp_to_date date) WITH (
     'connector.type' = 'dis',
     'connector.region' = 'xxxxx',
     'connector.channel' = 'dis-output',
     'format.type' = 'json'
   );
   /** Statistics on static car information**/
   INSERT
   INTO
     cars_infos_out
   SELECT
     cast(cast_int_to_string as string),
     cast(cast_String_to_int as int),
     cast(case_string_to_timestamp as timestamp),
     cast(case_timestamp_to_date as date)
   FROM
     car_infos;
