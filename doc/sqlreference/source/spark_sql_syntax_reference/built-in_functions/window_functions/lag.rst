:original_name: dli_spark_lag.html

.. _dli_spark_lag:

lag
===

This function is used to return the value of the *n*\ th row upwards within a specified window.

Restrictions
------------

The restrictions on using window functions are as follows:

-  Window functions can be used only in select statements.
-  Window functions and aggregate functions cannot be nested in window functions.
-  Window functions cannot be used together with aggregate functions of the same level.

Syntax
------

.. code-block::

   lag(<expr>[, bigint <offset>[, <default>]]) over([partition_clause] orderby_clause)

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

Groups all records by **cookieid**, sorts the records by **createtime** in ascending order, and returns the value of the second row above the window. An example command is as follows:

Example 1:

.. code-block::

   SELECT cookieid, createtime, url,
          LAG(createtime, 2) OVER (PARTITION BY cookieid ORDER BY createtime) AS last_2_time
   FROM logs;
   -- Returned result:
   cookieid createtime         url  last_2_time
   cookie1 2015-04-10 10:00:00 url1 NULL
   cookie1 2015-04-10 10:00:02 url2 NULL
   cookie1 2015-04-10 10:03:04 url3 2015-04-10 10:00:00
   cookie1 2015-04-10 10:10:00 url4 2015-04-10 10:00:02
   cookie1 2015-04-10 10:50:01 url5 2015-04-10 10:03:04
   cookie1 2015-04-10 10:50:05 url6 2015-04-10 10:10:00
   cookie1 2015-04-10 11:00:00 url7 2015-04-10 10:50:01
   cookie2 2015-04-10 10:00:00 url11 NULL
   cookie2 2015-04-10 10:00:02 url22 NULL
   cookie2 2015-04-10 10:03:04 url33 2015-04-10 10:00:00
   cookie2 2015-04-10 10:10:00 url44 2015-04-10 10:00:02
   cookie2 2015-04-10 10:50:01 url55 2015-04-10 10:03:04
   cookie2 2015-04-10 10:50:05 url66 2015-04-10 10:10:00
   cookie2 2015-04-10 11:00:00 url77 2015-04-10 10:50:01

.. note::

   Note: Because no default value is set, **NULL** is returned when the preceding two rows do not exist.

Example 2:

.. code-block::

   SELECT cookieid, createtime, url,
          LAG(createtime,1,'1970-01-01 00:00:00') OVER (PARTITION BY cookieid ORDER BY createtime) AS last_1_time
   FROM cookie4;
   -- Result:
   cookieid createtime          url last_1_time
   cookie1 2015-04-10 10:00:00 url1 1970-01-01 00:00:00 (The default value is displayed.)
   cookie1 2015-04-10 10:00:02 url2 2015-04-10 10:00:00
   cookie1 2015-04-10 10:03:04 url3 2015-04-10 10:00:02
   cookie1 2015-04-10 10:10:00 url4 2015-04-10 10:03:04
   cookie1 2015-04-10 10:50:01 url5 2015-04-10 10:10:00
   cookie1 2015-04-10 10:50:05 url6 2015-04-10 10:50:01
   cookie1 2015-04-10 11:00:00 url7 2015-04-10 10:50:05
   cookie2 2015-04-10 10:00:00 url11 1970-01-01 00:00:00 (The default value is displayed.)
   cookie2 2015-04-10 10:00:02 url22 2015-04-10 10:00:00
   cookie2 2015-04-10 10:03:04 url33 2015-04-10 10:00:02
   cookie2 2015-04-10 10:10:00 url44 2015-04-10 10:03:04
   cookie2 2015-04-10 10:50:01 url55 2015-04-10 10:10:00
   cookie2 2015-04-10 10:50:05 url66 2015-04-10 10:50:01
   cookie2 2015-04-10 11:00:00 url77 2015-04-10 10:50:05
