:original_name: dli_spark_current_date.html

.. _dli_spark_current_date:

current_date
============

This function is used to return the current date, in the **yyyy-mm-dd** format.

Similar function: :ref:`getdate <dli_spark_getdate>`. The getdate function is used to return the current system time, in the **yyyy-mm-dd hh:mi:ss** format.

Syntax
------

.. code-block::

   current_date()

Parameters
----------

None

Return Values
-------------

The return value is of the DATE type, in the **yyyy-mm-dd** format.

Example Code
------------

The value **2023-08-16** is returned.

.. code-block::

   select current_date();
