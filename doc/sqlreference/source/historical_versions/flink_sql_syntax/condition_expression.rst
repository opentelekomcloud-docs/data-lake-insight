:original_name: dli_08_0103.html

.. _dli_08_0103:

Condition Expression
====================

CASE Expression
---------------

**Syntax**

::

   CASE value WHEN value1 [, value11 ]* THEN result1
     [ WHEN valueN [, valueN1 ]* THEN resultN ]* [ ELSE resultZ ]
     END

or

::

   CASE WHEN condition1 THEN result1
     [ WHEN conditionN THEN resultN ]* [ ELSE resultZ ]
     END

**Description**

-  If the value of **value** is **value1**, **result1** is returned. If the value is not any of the values listed in the clause, **resultZ** is returned. If no else statement is specified, **null** is returned.
-  If the value of **condition1** is **true**, **result1** is returned. If the value does not match any condition listed in the clause, **resultZ** is returned. If no else statement is specified, **null** is returned.

**Precautions**

-  All results must be of the same type.
-  All conditions must be of the Boolean type.
-  If the value does not match any condition, the value of **ELSE** is returned when the else statement is specified, and **null** is returned when no else statement is specified.

**Example**

If the value of **units** equals **5**, **1** is returned. Otherwise, **0** is returned.

Example 1:

::

   insert into temp SELECT  CASE units WHEN 5 THEN 1 ELSE 0 END FROM Orders;

Example 2:

::

   insert into temp SELECT CASE WHEN units = 5 THEN 1 ELSE 0 END FROM Orders;

NULLIF Expression
-----------------

**Syntax**

::

   NULLIF(value, value)

**Description**

If the values are the same, **NULL** is returned. For example, **NULL** is returned from NULLIF (5,5) and **5** is returned from NULLIF (5,0).

**Precautions**

None

**Example**

If the value of **units** equals **3**, **null** is returned. Otherwise, the value of **units** is returned.

::

   insert into temp SELECT  NULLIF(units, 3) FROM Orders;

COALESCE Expression
-------------------

**Syntax**

::

   COALESCE(value, value [, value ]* )

**Description**

Return the first value that is not **NULL**, counting from left to right.

**Precautions**

All values must be of the same type.

**Example**

**5** is returned from the following example:

::

   insert into temp SELECT  COALESCE(NULL, 5) FROM Orders;
