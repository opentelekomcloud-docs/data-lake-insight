:original_name: dli_08_0101.html

.. _dli_08_0101:

Other Functions
===============

Array Functions
---------------

.. table:: **Table 1** Array functions

   +--------------------+------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Function           | Return Data Type | Description                                                                                                                                                                            |
   +====================+==================+========================================================================================================================================================================================+
   | CARDINALITY(ARRAY) | INT              | Return the element count of an array.                                                                                                                                                  |
   +--------------------+------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ELEMENT(ARRAY)     | ``-``            | Return the sole element of an array with a single element. If the array contains no elements, **null** is returned. If the array contains multiple elements, an exception is reported. |
   +--------------------+------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example:

The returned number of elements in the array is 3.

.. code-block::

   insert into temp select CARDINALITY(ARRAY[TRUE, TRUE, FALSE]) from source_stream;

**HELLO WORLD** is returned.

.. code-block::

   insert into temp select ELEMENT(ARRAY['HELLO WORLD']) from source_stream;

Attribute Access Functions
--------------------------

.. table:: **Table 2** Attribute access functions

   +-------------------------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Function                      | Return Data Type | Description                                                                                                                                                               |
   +===============================+==================+===========================================================================================================================================================================+
   | tableName.compositeType.field | ``-``            | Select a single field, use the name to access the field of Apache Flink composite types, such as Tuple and POJO, and return the value.                                    |
   +-------------------------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tableName.compositeType.\*    | ``-``            | Select all fields, and convert Apache Flink composite types, such as Tuple and POJO, and all their direct subtypes into a simple table. Each subtype is a separate field. |
   +-------------------------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
