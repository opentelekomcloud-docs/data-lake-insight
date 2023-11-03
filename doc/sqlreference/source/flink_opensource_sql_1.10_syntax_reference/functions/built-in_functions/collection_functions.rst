:original_name: dli_08_0337.html

.. _dli_08_0337:

Collection Functions
====================

Description
-----------

.. table:: **Table 1** Collection functions

   +-----------------------------------+------------------------------------------------------------------------+
   | Function                          | Description                                                            |
   +===================================+========================================================================+
   | CARDINALITY(array)                | Returns the number of elements in array.                               |
   +-----------------------------------+------------------------------------------------------------------------+
   | array '[' integer ']'             | Returns the element at position INT in array. The index starts from 1. |
   +-----------------------------------+------------------------------------------------------------------------+
   | ELEMENT(array)                    | Returns the sole element of array (whose cardinality should be one)    |
   |                                   |                                                                        |
   |                                   | Returns **NULL** if array is empty.                                    |
   |                                   |                                                                        |
   |                                   | Throws an exception if array has more than one element.                |
   +-----------------------------------+------------------------------------------------------------------------+
   | CARDINALITY(map)                  | Returns the number of entries in map.                                  |
   +-----------------------------------+------------------------------------------------------------------------+
   | map '[' key ']'                   | Returns the value specified by key value in map.                       |
   +-----------------------------------+------------------------------------------------------------------------+
