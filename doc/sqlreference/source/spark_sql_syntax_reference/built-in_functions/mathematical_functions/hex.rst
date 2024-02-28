:original_name: dli_spark_hex.html

.. _dli_spark_hex:

hex
===

This function is used to convert an integer or character into its hexadecimal representation.

Syntax
------

.. code-block::

   hex(BIGINT a)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type                               | Description                                                                                                       |
   +=================+=================+====================================+===================================================================================================================+
   | a               | Yes             | DOUBLE, BIGINT, DECIMAL, or STRING | The value can be a float, integer, or string.                                                                     |
   |                 |                 |                                    |                                                                                                                   |
   |                 |                 |                                    | If the value is not of the BIGINT type, the system will implicitly convert it to the BIGINT type for calculation. |
   |                 |                 |                                    |                                                                                                                   |
   |                 |                 |                                    | The string is converted to its corresponding ASCII code.                                                          |
   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **a** is **0**, **0** is returned.
   -  If the value of **a** is **NULL**, **NULL** is returned.

Example Code
------------

The value **0** is returned.

.. code-block::

   select hex(0);

The value **61** is returned.

.. code-block::

   select hex('a');

The value **10** is returned.

.. code-block::

   select hex(16);

The value **31** is returned.

.. code-block::

   select hex('1');

The value **3136** is returned.

.. code-block::

   select hex('16');

The value **NULL** is returned.

.. code-block::

   select hex(null);
