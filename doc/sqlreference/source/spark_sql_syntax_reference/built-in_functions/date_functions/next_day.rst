:original_name: dli_spark_next_day.html

.. _dli_spark_next_day:

next_day
========

This function is used to return the date closest to **day_of_week** after **start_date**.

Syntax
------

.. code-block::

   next_day(string start_date, string day_of_week)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                             |
   +=================+=================+=================+=========================================================================================================================================================+
   | start_date      | Yes             | DATE            | Date that needs to be processed                                                                                                                         |
   |                 |                 |                 |                                                                                                                                                         |
   |                 |                 | or              | If the value is of the STRING type, the value must contain at least yyyy-mm-dd and cannot contain redundant strings.                                    |
   |                 |                 |                 |                                                                                                                                                         |
   |                 |                 | STRING          | The following formats are supported:                                                                                                                    |
   |                 |                 |                 |                                                                                                                                                         |
   |                 |                 |                 | -  yyyy-mm-dd                                                                                                                                           |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss                                                                                                                                  |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss.ff3                                                                                                                              |
   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | day_of_week     | Yes             | STRING          | One day of a week, either the first two or three letters of the word, or the full name of the word for that day. For example, **TU** indicates Tuesday. |
   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DATE type, in the **yyyy-mm-dd** format.

.. note::

   -  If the value of **start_date** is not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the value of **start_date** is of the DATE or STRING type but is not in one of the supported formats, **NULL** is returned.
   -  If the value of **start_date** is **NULL**, **NULL** is returned.
   -  If the value of **day_of_week** is **NULL**, **NULL** is returned.

Example Code
------------

The value **2023-08-22** is returned.

.. code-block::

   select next_day('2023-08-16','TU');

The value **2023-08-22** is returned.

.. code-block::

   select next_day('2023-08-16 10:54:00','TU');

The value **2023-08-23** is returned.

.. code-block::

   select next_day('2023-08-16 10:54:00','WE');

The value **NULL** is returned.

.. code-block::

   select next_day('20230816','TU');

The value **NULL** is returned.

.. code-block::

   select next_day('20230816 20:00:00',null);
