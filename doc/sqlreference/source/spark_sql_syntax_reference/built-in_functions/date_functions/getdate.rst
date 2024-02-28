:original_name: dli_spark_getdate.html

.. _dli_spark_getdate:

getdate
=======

This function is used to return the current system time, in the **yyyy-mm-dd hh:mi:ss** format.

Similar function: :ref:`current_date <dli_spark_current_date>`. The **current_date** function is used to return the current date, in the **yyyy-mm-dd** format.

Syntax
------

.. code-block::

   getdate()

Parameters
----------

None

Return Values
-------------

The return value is of the STRING type, in the **yyyy-mm-dd hh:mi:ss** format.

Example Code
------------

If the current time is **2023-08-10 10:54:00**, **2023-08-10 10:54:00** is returned.

.. code-block::

   select getdate();
