:original_name: dli_spark_rank.html

.. _dli_spark_rank:

rank
====

This function is used to return the rank of a value in a set of values. When multiple values share the same rank, the next rank in the sequence is not consecutive.

Restrictions
------------

The restrictions on using window functions are as follows:

-  Window functions can be used only in select statements.
-  Window functions and aggregate functions cannot be nested in window functions.
-  Window functions cannot be used together with aggregate functions of the same level.

Syntax
------

.. code-block::

   rank() over ([partition_clause] [orderby_clause])

Parameters
----------

.. table:: **Table 1** Parameters

   +------------------+-----------+---------------------------------------------------------------------------------------------------+
   | Parameter        | Mandatory | Description                                                                                       |
   +==================+===========+===================================================================================================+
   | partition_clause | No        | Partition. Rows with the same value in partition columns are considered to be in the same window. |
   +------------------+-----------+---------------------------------------------------------------------------------------------------+
   | orderby_clause   | No        | It is used to specify how data is sorted in a window.                                             |
   +------------------+-----------+---------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the INT type.

.. note::

   If the value of **a** is **NULL**, **NULL** is returned.

Example Code
------------

To help you understand how to use functions, this example provides source data and function examples based on the source data. Run the following command to create the **logs** table and add data:

.. code-block::

   CREATE TABLE logs (
   cookieid string,
   createtime string,
   pv INT
   ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
   stored as textfile;

Adds the following data:

.. code-block::

   cookie1 2015-04-10 1
   cookie1 2015-04-11 5
   cookie1 2015-04-12 7
   cookie1 2015-04-13 3
   cookie1 2015-04-14 2
   cookie1 2015-04-15 4
   cookie1 2015-04-16 4
   cookie2 2015-04-10 2
   cookie2 2015-04-11 3
   cookie2 2015-04-12 5
   cookie2 2015-04-13 6
   cookie2 2015-04-14 3
   cookie2 2015-04-15 9
   cookie2 2015-04-16 7

Example: Groups all records by **cookieid**, sorts them by **pv** in descending order, and returns the sequence number of each row in the group. An example command is as follows:

.. code-block::

   select cookieid, createtime, pv,
          rank() over(partition by cookieid order by pv desc) as rank
   from logs
   where cookieid = 'cookie1';
   -- Result:
   cookie1 2015-04-12 7 1
   cookie1 2015-04-11 5 2
   cookie1 2015-04-16 4 3 (third in parallel)
   cookie1 2015-04-15 4 3
   cookie1 2015-04-13 3 5 (skip 4 and start from 5)
   cookie1 2015-04-14 2 6
   cookie1 2015-04-10 1 7
