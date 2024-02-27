:original_name: dli_spark_trans_array.html

.. _dli_spark_trans_array:

trans_array
===========

This function is used to convert an array split by a fixed separator in a column into multiple rows.

Restrictions
------------

-  All columns used as keys must be placed before the columns to be transposed.
-  Only one UDTF is allowed in a select statement.
-  This function cannot be used together with **group by**, **cluster by**, **distribute by**, or sort by.

Syntax
------

.. code-block::

   trans_array (<num_keys>, <separator>, <key1>,<key2>,…,<col1>,<col2>,<col3>) as (<key1>,<key2>,...,<col1>, <col2>)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------+-----------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type   | Description                                                                                                                                                                                                                                     |
   +===========+===========+========+=================================================================================================================================================================================================================================================+
   | num_keys  | Yes       | BIGINT | The value is a constant of the BIGINT type and must be greater than or equal to 0. This parameter indicates the number of columns that are used as transposed keys when being converted to multiple rows.                                       |
   +-----------+-----------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | separator | Yes       | STRING | The value is a constant of the STRING type, which is used to split a string into multiple elements. If this parameter is left blank, an error is reported.                                                                                      |
   +-----------+-----------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | keys      | Yes       | STRING | Columns used as keys during transpose. The number of columns is specified by **num_keys**. If **num_keys** specifies that all columns are used as keys (that is, **num_keys** is equal to the number of all columns), only one row is returned. |
   +-----------+-----------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cols      | Yes       | STRING | Array to be converted to rows. All columns following **keys** are regarded as arrays to be transposed and must be of the STRING type.                                                                                                           |
   +-----------+-----------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the data type of the parameter.

.. note::

   -  Transposed rows are returned. The new column name is specified by **as**.
   -  The type of the key column does not change, and the type of other columns is **STRING**.
   -  The number of rows after the split is subject to the array with more rows. If there are not enough rows, **NULL** is added.

Example Code
------------

To help you understand how to use functions, this example provides source data and function examples based on the source data. Run the following command to create the salary table and add data:

.. code-block::

   CREATE EXTERNAL TABLE salary (
   dept_id STRING, -- Department
   userid string, -- Employee ID
   sal INT -- Salary
   ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
   stored as textfile;

Adds the following data:

.. code-block::

   d1,user1/user4,1000/6000
   d1,user2/user5,2000/7000
   d1,user3/user6,3000
   d2,user4/user7,4000
   d2,user5/user8,5000/8000

Executes the SQL statement

.. code-block::

   select trans_array(1, "/", dept_id, user_id, sal) as (dept_id, user_id, sal) from salary;

The command output is as follows:

.. code-block::

   d1,user1,1000
   d1,user4,6000
   d1,user2,2000
   d1,user5,7000
   d1,user3,3000
   d1,user6,NULL
   d2,user4,4000
   d2,user7,NULL
   d2,user5,5000
   d2,user8,8000
