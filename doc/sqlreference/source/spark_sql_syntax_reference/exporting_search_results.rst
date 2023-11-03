:original_name: dli_08_0205.html

.. _dli_08_0205:

Exporting Search Results
========================

Function
--------

This statement is used to directly write query results to a specified directory. The query results can be stored in CSV, Parquet, ORC, JSON, or Avro format.

Syntax
------

::

   INSERT OVERWRITE DIRECTORY path
     USING file_format
     [OPTIONS(key1=value1)]
     select_statement;

Keyword
-------

-  USING: Specifies the storage format.
-  OPTIONS: Specifies the list of attributes to be exported. This parameter is optional.

Parameter
---------

.. table:: **Table 1** INSERT OVERWRITE DIRECTORY parameter description

   +-------------+--------------------------------------------------------------------------------------+
   | Parameter   | Description                                                                          |
   +=============+======================================================================================+
   | path        | The OBS path to which the query result is to be written.                             |
   +-------------+--------------------------------------------------------------------------------------+
   | file_format | Format of the file to be written. The value can be CSV, Parquet, ORC, JSON, or Avro. |
   +-------------+--------------------------------------------------------------------------------------+

.. note::

   If the file format is set to **CSV**, see the :ref:`Table 3 <dli_08_0076__en-us_topic_0114776170_table1876517231928>` for the OPTIONS parameters.

Precautions
-----------

-  You can configure the **spark.sql.shuffle.partitions** parameter to set the number of files to be inserted into the OBS bucket in the non-DLI table. In addition, to avoid data skew, you can add **distribute by rand()** to the end of the INSERT statement to increase the number of concurrent jobs. The following is an example:

   .. code-block::

      insert into table table_target select * from table_source distribute by cast(rand() * N as int);

-  When the configuration item is **OPTIONS('DELIMITER'=',')**, you can specify a separator. The default value is **,**.

   For CSV data, the following delimiters are supported:

   -  Tab character, for example, **'DELIMITER'='\\t'**.
   -  Any binary character, for example, **'DELIMITER'='\\u0001(^A)'**.
   -  Single quotation mark ('). A single quotation mark must be enclosed in double quotation marks (" "). For example, **'DELIMITER'= "'"**.
   -  **\\001(^A)** and **\\017(^Q)** are also supported, for example, **'DELIMITER'='\\001(^A)'** and **'DELIMITER'='\\017(^Q)'**.

Example
-------

::

   INSERT OVERWRITE DIRECTORY 'obs://bucket/dir'
     USING csv
     OPTIONS(key1=value1)
     select * from db1.tb1;
