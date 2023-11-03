:original_name: dli_08_0062.html

.. _dli_08_0062:

Arithmetic Operators
====================

Arithmetic operators include binary operators and unary operators. For both types of operators, the returned results are numbers. :ref:`Table 1 <dli_08_0062__en-us_topic_0093946771_tbd954815bc3c4c6f871f2536e1a13c23>` lists the arithmetic operators supported by DLI.

.. _dli_08_0062__en-us_topic_0093946771_tbd954815bc3c4c6f871f2536e1a13c23:

.. table:: **Table 1** Arithmetic operators

   +----------+-------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Operator | Result Type       | Description                                                                                                                                                                                      |
   +==========+===================+==================================================================================================================================================================================================+
   | A + B    | All numeric types | A plus B. The result type is associated with the operation data type. For example, if floating-point number is added to an integer, the result will be a floating-point number.                  |
   +----------+-------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | A-B      | All numeric types | A minus B. The result type is associated with the operation data type.                                                                                                                           |
   +----------+-------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | A \* B   | All numeric types | Multiply A and B. The result type is associated with the operation data type.                                                                                                                    |
   +----------+-------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | A / B    | All numeric types | Divide A by B. The result is a number of the double type (double-precision number).                                                                                                              |
   +----------+-------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | A % B    | All numeric types | A on the B Modulo. The result type is associated with the operation data type.                                                                                                                   |
   +----------+-------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | A & B    | All numeric types | Check the value of the two parameters in binary expressions and perform the AND operation by bit. If the same bit of both expressions are 1, then the bit is set to 1. Otherwise, the bit is 0.  |
   +----------+-------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | A \| B   | All numeric types | Check the value of the two parameters in binary expressions and perform the OR operation by bit. If one bit of either expression is 1, then the bit is set to 1. Otherwise, the bit is set to 0. |
   +----------+-------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | A ^ B    | All numeric types | Check the value of the two parameters in binary expressions and perform the XOR operation by bit. Only when one bit of either expression is 1, the bit is 1. Otherwise, the bit is 0.            |
   +----------+-------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ~A       | All numeric types | Perform the NOT operation on one expression by bit.                                                                                                                                              |
   +----------+-------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
