:original_name: dli_08_15078.html

.. _dli_08_15078:

OrderBy & Limit
===============

OrderBy
-------

**Function**

This clause is used to sort data in ascending order on a time attribute.

**Precautions**

Currently, only sorting by time attribute is supported.

**Example**

Sort data in ascending order on the time attribute.

.. code-block::

   SELECT *
   FROM Orders
   ORDER BY orderTime;

Limit
-----

**Function**

This clause is used to constrain the number of rows returned.

**Precautions**

This clause is used in conjunction with ORDER BY to ensure that the results are deterministic.

**Example**

.. code-block::

   SELECT *
   FROM Orders
   ORDER BY orderTime
   LIMIT 3;
