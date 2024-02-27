:original_name: dli_spark_add_months.html

.. _dli_spark_add_months:

add_months
==========

This function is used to calculate the date after a date value is increased by a specified number of months. That is, it calculates the data that is **num_months** after **start_date**.

Syntax
------

.. code-block::

   add_months(string start_date, int num_months)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+--------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                          |
   +=================+=================+=================+======================================+
   | start_date      | Yes             | DATE or STRING  | Start date                           |
   |                 |                 |                 |                                      |
   |                 |                 |                 | The following formats are supported: |
   |                 |                 |                 |                                      |
   |                 |                 |                 | -  yyyy-mm-dd                        |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss               |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss.ff3           |
   +-----------------+-----------------+-----------------+--------------------------------------+
   | num_months      | Yes             | INT             | Number of months to be added         |
   +-----------------+-----------------+-----------------+--------------------------------------+

Return Values
-------------

The date that is **num_months** after **start_date** is returned, in the **yyyy-mm-dd** format.

The return value is of the DATE type.

.. note::

   -  If the value of **start_date** is not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the value of **start_date** is of the DATE or STRING type but is not in one of the supported formats, **NULL** is returned.
   -  If the value of **start_date** is **NULL**, an error is reported.
   -  If the value of **num_months** is **NULL**, **NULL** is returned.

Example Code
------------

The value **2023-05-26** is returned.

.. code-block::

   select add_months('2023-02-26',3);

The value **2023-05-14** is returned.

.. code-block::

   select add_months('2023-02-14 21:30:00',3);

The value **NULL** is returned.

.. code-block::

   select add_months('20230815',3);

The value **NULL** is returned.

.. code-block::

   select add_months('2023-08-15 20:00:00',null);
