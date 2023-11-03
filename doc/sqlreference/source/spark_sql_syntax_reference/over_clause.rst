:original_name: dli_08_0190.html

.. _dli_08_0190:

OVER Clause
===========

Function
--------

This statement is used together with the window function. The OVER statement is used to group data and sort the data within the group. The window function is used to generate serial numbers for values within the group.

Syntax
------

::

   SELECT window_func(args) OVER
     ([PARTITION BY col_name, col_name, ...]
      [ORDER BY col_name, col_name, ...]
      [ROWS | RANGE BETWEEN (CURRENT ROW | (UNBOUNDED |[num]) PRECEDING)
     AND (CURRENT ROW | ( UNBOUNDED | [num]) FOLLOWING)]);

Keyword
-------

-  PARTITION BY: used to partition a table with one or multiple fields. Similar to GROUP BY, PARTITION BY is used to partition table by fields and each partition is a window. The window function can apply to the entire table or specific partitions. A maximum of 7,000 partitions can be created in a single table.
-  ORDER BY: used to specify the order for the window function to obtain the value. ORDER BY can be used to sort table with one or multiple fields. The sorting order can be ascending (specified by **ASC**) or descending (specified by **DESC**). The window is specified by WINDOW. If the window is not specified, the default window is ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW. In other words, the window starts from the head of the table or partition (if PARTITION BY is used in the OVER clause) to the current row.
-  WINDOW: used to define the window by specifying a range of rows.
-  CURRENT ROW: indicates the current row.
-  num PRECEDING: used to specify the start of the defined window. The window starts from the num row precedes the current row.
-  UNBOUNDED PRECEDING: used to indicate that there is no start of the window.
-  num FOLLOWING: used to specify the end of the defined window. The window ends from the num row following the current row.
-  UNBOUNDED FOLLOWING: used to indicate that there is no end of the window.
-  The differences between ROWS BETWEEN... and RANGE BETWEEN... are as follows:

   -  ROWS refers to the physical window. After the data is sorted, the physical window starts at the *n*\ th row in front of the current row and ends at the *m*\ th row following the current row.
   -  RANGE refers to the logic window. The column of the logic window is determined by the values rather than the location of rows.

-  The scenarios of the window are as follows:

   -  The window only contains the current row.

      ::

         ROWS BETWEEN CURRENT ROW AND CURRENT ROW

   -  The window starts from three rows precede the current row and ends at the fifth row follows the current row.

      ::

         ROWS BETWEEN 3 PRECEDING AND 5 FOLLOWING

   -  The window starts from the beginning of the table or partition and ends at the current row.

      ::

         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW

   -  The window starts from the current window and ends at the end of the table or partition.

      ::

         ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING

   -  The window starts from the beginning of the table or partition and ends at the end of the table or partition.

      ::

         ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING

Precautions
-----------

The three options of the OVER clause are PARTITION BY, ORDER BY, and WINDOW. They are optional and can be used together. If the OVER clause is empty, the window is the entire table.

Example
-------

To start the window from the beginning of the table or partition and end the window at the current row, sort the over_test table according to the id field, and return the sorted id fields and corresponding serial numbers, run the following statement:

::

   SELECT id, count(id) OVER (ORDER BY id ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) FROM over_test;
