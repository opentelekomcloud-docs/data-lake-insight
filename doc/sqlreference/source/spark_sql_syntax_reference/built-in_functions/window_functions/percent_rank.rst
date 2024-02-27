:original_name: dli_spark_percent_rank.html

.. _dli_spark_percent_rank:

percent_rank
============

This function is used to return the value of the column specified in the ORDER BY clause of a window, expressed as a decimal between 0 and 1. It is calculated as (the rank value of the current row within the group - 1) divided by (the total number of rows in the group - 1).

Restrictions
------------

The restrictions on using window functions are as follows:

-  Window functions can be used only in select statements.
-  Window functions and aggregate functions cannot be nested in window functions.
-  Window functions cannot be used together with aggregate functions of the same level.

Syntax
------

.. code-block::

   percent_rank() over([partition_clause] [orderby_clause])

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

The return value is of the DOUBLE type.

Example Code
------------

**Example data**

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

Example: Calculates the percentage ranking of employees' salaries in a department.

.. code-block::

   select dept, userid, sal,
          percent_rank() over(partition by dept order by sal) as pr2
   from salary;
   -- Result analysis:
   d1 user1 1000 0.0    -- (1-1)/(3-1)=0.0
   d1 user2 2000 0.5    -- (2-1)/(3-1)=0.5
   d1 user3 3000 1.0    -- (3-1)/(3-1)=1.0
   d2 user4 4000 0.0    -- (1-1)/(2-1)=0.0
   d2 user5 5000 1.0    -- (2-1)/(2-1)=1.0
