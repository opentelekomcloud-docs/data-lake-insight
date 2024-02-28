:original_name: dli_spark_cume_dist.html

.. _dli_spark_cume_dist:

cume_dist
=========

This function is used to return the cumulative distribution, which is equivalent to calculating the proportion of data in the partition that is greater than or equal to, or less than or equal to, the current row.

Restrictions
------------

The restrictions on using window functions are as follows:

-  Window functions can be used only in select statements.
-  Window functions and aggregate functions cannot be nested in window functions.
-  Window functions cannot be used together with aggregate functions of the same level.

Syntax
------

.. code-block::

   cume_dist() over([partition_clause] [orderby_clause])

Parameters
----------

.. table:: **Table 1** Parameters

   +------------------+-----------+---------------------------------------------------------------------------------------------------+
   | Parameter        | Mandatory | Description                                                                                       |
   +==================+===========+===================================================================================================+
   | partition_clause | No        | Partition. Rows with the same value in partition columns are considered to be in the same window. |
   +------------------+-----------+---------------------------------------------------------------------------------------------------+
   | orderby_clause   | No        | How data is sorted in a window                                                                    |
   +------------------+-----------+---------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

.. note::

   If the value of **a** is **NULL**, **NULL** is returned.

Example Code
------------

To help you understand how to use functions, this example provides source data and function examples based on the source data. Run the following command to create the salary table and add data:

.. code-block::

   CREATE EXTERNAL TABLE salary (
   dept STRING, -- Department name
   userid string, -- Employee ID
   sal INT -- Salary
   ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
   stored as textfile;

Adds the following data:

.. code-block::

   d1,user1,1000
   d1,user2,2000
   d1,user3,3000
   d2,user4,4000
   d2,user5,5000

-  Calculates the proportion of employees whose salary is less than or equal to the current salary.

   .. code-block::

      select dept, userid, sal,
             cume_dist() over(order by sal) as cume1
      from salary;
      -- Result:
      d1 user1 1000 0.2
      d1 user2 2000 0.4
      d1 user3 3000 0.6
      d2 user4 4000 0.8
      d2 user5 5000 1.0

-  Calculates the proportion of employees whose salary is less than or equal to the current salary by department.

   .. code-block::

      select dept, userid, sal,
             cume_dist() over (partition by dept order by sal) as cume2
      from salary;
      -- Result:
      d1 user1 1000 0.3333333333333333
      d1 user2 2000 0.6666666666666666
      d1 user3 3000 1.0
      d2 user4 4000 0.5
      d2 user5 5000 1.0

-  After sorting by **sal** in descending order, the result is the ratio of employees whose salary is greater than or equal to the current salary.

   .. code-block::

      select dept, userid, sal,
             cume_dist() over(order by sal desc) as cume3
      from salary;
      -- Result:
      d2 user5 5000 0.2
      d2 user4 4000 0.4
      d1 user3 3000 0.6
      d1 user2 2000 0.8
      d1 user1 1000 1.0
      select dept, userid, sal,
             cume_dist() over(partition by dept order by sal desc) as cume4
      from salary;
      -- Result:
      d1 user3 3000 0.3333333333333333
      d1 user2 2000 0.6666666666666666
      d1 user1 1000 1.0
      d2 user5 5000 0.5
      d2 user4 4000 1.0
