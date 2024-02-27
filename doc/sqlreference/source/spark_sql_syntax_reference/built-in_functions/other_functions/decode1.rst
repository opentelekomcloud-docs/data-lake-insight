:original_name: dli_spark_decode1.html

.. _dli_spark_decode1:

decode1
=======

This function is used to implement if-then-else branch selection.

Syntax
------

.. code-block::

   decode1(<expression>, <search>, <result>[, <search>, <result>]...[, <default>])

Parameters
----------

.. table:: **Table 1** Parameters

   +------------+-----------+--------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type                           | Description                                                                                                                      |
   +============+===========+================================+==================================================================================================================================+
   | expression | Yes       | All data types                 | Expression to be compared                                                                                                        |
   +------------+-----------+--------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
   | search     | Yes       | Same as that of **expression** | Search item to be compared with **expression**                                                                                   |
   +------------+-----------+--------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
   | result     | Yes       | All data types                 | Return value when the values of **search** and **expression** match                                                              |
   +------------+-----------+--------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
   | default    | No        | Same as that of **result**     | If all search items do not match, the value of this parameter is returned. If no search item is specified, **NULL** is returned. |
   +------------+-----------+--------------------------------+----------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

**result** and **default** are return values. These values can be of any data type.

.. note::

   -  If they match, the value of **result** is returned.
   -  If no match is found, the value of **default** is returned.
   -  If **default** is not specified, **NULL** is returned.
   -  If the search options are duplicate and matched, the first value is returned.

Example Code
------------

To help you understand how to use functions, this example provides source data and function examples based on the source data. Run the following command to create the salary table and add data:

.. code-block::

   CREATE EXTERNAL TABLE salary (
   dept_id STRING, -- Department
   userid string, -- Employee ID
   sal INT
   ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
   stored as textfile;

Adds the following data:

.. code-block::

   d1,user1,1000
   d1,user2,2000
   d1,user3,3000
   d2,user4,4000
   d2,user5,5000

**Example**

Returns the name of each department.

If **dept_id** is set to **d1**, **DLI** is returned. If it is set to **d2**, **MRS** is returned. In other scenarios, **Others** is returned.

.. code-block::

   select dept, decode1(dept, 'd1', 'DLI', 'd2', 'MRS', 'Others') as result from sale_detail;

Returned result:

.. code-block::

   d1 DLI
   d2 MRS
   d3 Others
   d4 Others
   d5 Others
