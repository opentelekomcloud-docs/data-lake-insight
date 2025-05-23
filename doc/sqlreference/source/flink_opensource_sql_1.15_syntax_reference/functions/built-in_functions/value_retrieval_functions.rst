:original_name: dli_08_15096.html

.. _dli_08_15096:

Value Retrieval Functions
=========================

.. table:: **Table 1** Value retrieval functions

   +-------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SQL Function                  | Description                                                                                                                                                                                                                                                                                               |
   +===============================+===========================================================================================================================================================================================================================================================================================================+
   | tableName.compositeType.field | Returns the value of a field from a Flink composite type (e.g. Tuple, POJO) by name.                                                                                                                                                                                                                      |
   +-------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tableName.compositeType.\*    | Returns the flattened representation of a Flink composite type (e.g. Tuple, POJO), converting each of its immediate subtypes into a separate field. In most cases, the fields in the flattened representation have similar names to the original fields, but with a $ separator (e.g. mypojo$mytuple$f0). |
   +-------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
