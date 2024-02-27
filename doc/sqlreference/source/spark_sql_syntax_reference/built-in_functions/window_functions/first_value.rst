:original_name: dli_spark_first_value.html

.. _dli_spark_first_value:

first_value
===========

This function is used to obtain the value of the first data record in the window corresponding to the current row.

Restrictions
------------

The restrictions on using window functions are as follows:

-  Window functions can be used only in select statements.
-  Window functions and aggregate functions cannot be nested in window functions.
-  Window functions cannot be used together with aggregate functions of the same level.

Syntax
------

.. code-block::

   first_value(<expr>[, <ignore_nulls>]) over ([partition_clause] [orderby_clause] [frame_clause])

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                 |
   +=======================+=======================+=============================================================================================================+
   | expr                  | Yes                   | Expression whose return result is to be calculated                                                          |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------+
   | ignore_nulls          | No                    | The value is of the BOOLEAN type, indicating whether to ignore NULL values. The default value is **False**. |
   |                       |                       |                                                                                                             |
   |                       |                       | If the value is **True**, the first non-null value in the window is returned.                               |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------+
   | partition_clause      | No                    | Partition. Rows with the same value in partition columns are considered to be in the same window.           |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------+
   | orderby_clause        | No                    | It is used to specify how data is sorted in a window.                                                       |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------+
   | frame_clause          | No                    | It is used to determine the data boundary.                                                                  |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------+

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

Example: Groups all records by **cookieid**, sorts the records by **createtime** in ascending order, and returns the first row of data in each group. An example command is as follows:

.. code-block::

   SELECT cookieid, createtime, url,
          FIRST_VALUE(url) OVER (PARTITION BY cookieid ORDER BY createtime) AS first
   FROM logs;

   The command output is as follows:
   cookieid createtime         url  first
   cookie1 2015-04-10 10:00:00 url1 url1
   cookie1 2015-04-10 10:00:02 url2 url1
   cookie1 2015-04-10 10:03:04 url3 url1
   cookie1 2015-04-10 10:10:00 url4 url1
   cookie1 2015-04-10 10:50:01 url5 url1
   cookie1 2015-04-10 10:50:05 url6 url1
   cookie1 2015-04-10 11:00:00 url7 url1
   cookie2 2015-04-10 10:00:00 url11 url11
   cookie2 2015-04-10 10:00:02 url22 url11
   cookie2 2015-04-10 10:03:04 url33 url11
   cookie2 2015-04-10 10:10:00 url44 url11
   cookie2 2015-04-10 10:50:01 url55 url11
   cookie2 2015-04-10 10:50:05 url66 url11
   cookie2 2015-04-10 11:00:00 url77 url11
