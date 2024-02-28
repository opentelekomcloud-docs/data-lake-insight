:original_name: dli_spark_lead.html

.. _dli_spark_lead:

lead
====

This function is used to return the value of the *n*\ th row downwards within a specified window.

Restrictions
------------

The restrictions on using window functions are as follows:

-  Window functions can be used only in select statements.
-  Window functions and aggregate functions cannot be nested in window functions.
-  Window functions cannot be used together with aggregate functions of the same level.

Syntax
------

.. code-block::

   lead(<expr>[, bigint <offset>[, <default>]]) over([partition_clause] orderby_clause)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                                                                                                                                                                                                  |
   +=======================+=======================+==============================================================================================================================================================================================================================================================================================================================================+
   | expr                  | Yes                   | Expression whose return result is to be calculated                                                                                                                                                                                                                                                                                           |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | offset                | No                    | Offset. It is a constant of the BIGINT type and its value is greater than or equal to 0. The value **0** indicates the current row, the value **1** indicates the previous row, and so on. The default value is **1**. If the input value is of the STRING or DOUBLE type, it is implicitly converted to the BIGINT type before calculation. |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | default               | Yes                   | Constant. The default value is **NULL**.                                                                                                                                                                                                                                                                                                     |
   |                       |                       |                                                                                                                                                                                                                                                                                                                                              |
   |                       |                       | Default value when the range specified by **offset** is out of range. The value must be the same as the data type corresponding to **expr**. If **expr** is non-constant, the evaluation is performed based on the current row.                                                                                                              |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | partition_clause      | No                    | Partition. Rows with the same value in partition columns are considered to be in the same window.                                                                                                                                                                                                                                            |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | orderby_clause        | No                    | It is used to specify how data is sorted in a window.                                                                                                                                                                                                                                                                                        |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the data type of the parameter.

Example Code
------------

**Example data**

To help you understand how to use functions, this example provides source data and function examples based on the source data. Run the following command to create the **logs** table and add data:

.. code-block::

   create table logs(
    cookieid string,
    createtime string,
    url string
   )
   STORED AS parquet;

Adds the following data:

.. code-block::

   cookie1 2015-04-10 10:00:02 url2
   cookie1 2015-04-10 10:00:00 url1
   cookie1 2015-04-10 10:03:04 url3
   cookie1 2015-04-10 10:50:05 url6
   cookie1 2015-04-10 11:00:00 url7
   cookie1 2015-04-10 10:10:00 url4
   cookie1 2015-04-10 10:50:01 url5
   cookie2 2015-04-10 10:00:02 url22
   cookie2 2015-04-10 10:00:00 url11
   cookie2 2015-04-10 10:03:04 url33
   cookie2 2015-04-10 10:50:05 url66
   cookie2 2015-04-10 11:00:00 url77
   cookie2 2015-04-10 10:10:00 url44
   cookie2 2015-04-10 10:50:01 url55

Groups all records by **cookieid**, sorts them by **createtime** in ascending order, and returns the values of the second and first rows downwards within the specified window. An example command is as follows:

.. code-block::

   SELECT cookieid, createtime, url,
          LEAD(createtime, 2) OVER(PARTITION BY cookieid ORDER BY createtime) AS next_2_time,
          LEAD(createtime, 1, '1970-01-01 00:00:00') OVER(PARTITION BY cookieid ORDER BY createtime) AS next_1_time
   FROM logs;

   -- Returned result:
   cookieid createtime         url  next_2_time          next_1_time
   cookie1 2015-04-10 10:00:00 url1 2015-04-10 10:03:04  2015-04-10 10:00:02
   cookie1 2015-04-10 10:00:02 url2 2015-04-10 10:10:00  2015-04-10 10:03:04
   cookie1 2015-04-10 10:03:04 url3 2015-04-10 10:50:01  2015-04-10 10:10:00
   cookie1 2015-04-10 10:10:00 url4 2015-04-10 10:50:05  2015-04-10 10:50:01
   cookie1 2015-04-10 10:50:01 url5 2015-04-10 11:00:00  2015-04-10 10:50:05
   cookie1 2015-04-10 10:50:05 url6 NULL                 2015-04-10 11:00:00
   cookie1 2015-04-10 11:00:00 url7 NULL                 1970-01-01 00:00:00
   cookie2 2015-04-10 10:00:00 url11 2015-04-10 10:03:04 2015-04-10 10:00:02
   cookie2 2015-04-10 10:00:02 url22 2015-04-10 10:10:00 2015-04-10 10:03:04
   cookie2 2015-04-10 10:03:04 url33 2015-04-10 10:50:01 2015-04-10 10:10:00
   cookie2 2015-04-10 10:10:00 url44 2015-04-10 10:50:05 2015-04-10 10:50:01
   cookie2 2015-04-10 10:50:01 url55 2015-04-10 11:00:00 2015-04-10 10:50:05
   cookie2 2015-04-10 10:50:05 url66 NULL                2015-04-10 11:00:00
   cookie2 2015-04-10 11:00:00 url77 NULL                1970-01-01 00:00:00
