:original_name: dli_08_0102.html

.. _dli_08_0102:

SELECT
======


SELECT
------

**Syntax**

::

   SELECT [ ALL | DISTINCT ]  { * | projectItem [, projectItem ]* }
     FROM tableExpression
     [ WHERE booleanExpression ]
     [ GROUP BY { groupItem [, groupItem ]* } ]
     [ HAVING booleanExpression ]

**Description**

The SELECT statement is used to select data from a table or insert constant data into a table.

**Precautions**

-  The table to be queried must exist. Otherwise, an error is reported.
-  WHERE is used to specify the filtering condition, which can be the arithmetic operator, relational operator, or logical operator.
-  GROUP BY is used to specify the grouping field, which can be one or more multiple fields.

**Example**

Select the order which contains more than 3 pieces of data.

::

   insert into temp SELECT  * FROM Orders WHERE units > 3;

Insert a group of constant data.

::

   insert into temp select 'Lily', 'male', 'student', 17;

WHERE Filtering Clause
----------------------

**Syntax**

::

   SELECT   { * | projectItem [, projectItem ]* }
     FROM tableExpression
     [ WHERE booleanExpression ]

**Description**

This statement is used to filter the query results using the WHERE clause.

**Precautions**

-  The to-be-queried table must exist.
-  WHERE filters the records that do not meet the requirements.

**Example**

Filter orders which contain more than 3 pieces and fewer than 10 pieces of data.

::

   insert into temp SELECT  * FROM Orders
     WHERE units > 3 and units < 10;

HAVING Filtering Clause
-----------------------

**Function**

This statement is used to filter the query results using the HAVING clause.

**Syntax**

::

   SELECT [ ALL | DISTINCT ]   { * | projectItem [, projectItem ]* }
     FROM tableExpression
     [ WHERE booleanExpression ]
     [ GROUP BY { groupItem [, groupItem ]* } ]
     [ HAVING booleanExpression ]

**Description**

Generally, HAVING and GROUP BY are used together. GROUP BY applies first for grouping and HAVING then applies for filtering. The arithmetic operation and aggregate function are supported by the HAVING clause.

**Precautions**

If the filtering condition is subject to the query results of GROUP BY, the HAVING clause, rather than the WHERE clause, must be used for filtering.

**Example**

Group the **student** table according to the **name** field and filter the records in which the maximum score is higher than 95 based on groups.

::

   insert into temp SELECT name, max(score) FROM student
     GROUP BY name
     HAVING max(score) >95

Column-Based GROUP BY
---------------------

**Function**

This statement is used to group a table based on columns.

**Syntax**

::

   SELECT [ ALL | DISTINCT ]   { * | projectItem [, projectItem ]* }
     FROM tableExpression
     [ WHERE booleanExpression ]
     [ GROUP BY { groupItem [, groupItem ]* } ]

**Description**

Column-based GROUP BY can be categorized into single-column GROUP BY and multi-column GROUP BY.

-  Single-column GROUP BY indicates that the GROUP BY clause contains only one column.
-  Multi-column GROUP BY indicates that the GROUP BY clause contains multiple columns. The table will be grouped according to all fields in the GROUP BY clause. The records whose fields are the same are grouped into one group.

**Precautions**

None

**Example**

Group the **student** table according to the score and name fields and return the grouping results.

::

   insert into temp SELECT name,score, max(score) FROM student
     GROUP BY name,score;

Expression-Based GROUP BY
-------------------------

**Function**

This statement is used to group a table according to expressions.

**Syntax**

::

   SELECT [ ALL | DISTINCT ]   { * | projectItem [, projectItem ]* }
     FROM tableExpression
     [ WHERE booleanExpression ]
     [ GROUP BY { groupItem [, groupItem ]* } ]

**Description**

groupItem can have one or more fields. The fields can be called by string functions, but cannot be called by aggregate functions.

**Precautions**

None

**Example**

Use the substring function to obtain the string from the name field, group the **student** table according to the obtained string, and return each sub string and the number of records.

::

   insert into temp SELECT substring(name,6),count(name) FROM student
     GROUP BY substring(name,6);

GROUP BY Using HAVING
---------------------

**Function**

This statement filters a table after grouping it using the HAVING clause.

**Syntax**

::

   SELECT [ ALL | DISTINCT ]   { * | projectItem [, projectItem ]* }
     FROM tableExpression
     [ WHERE booleanExpression ]
     [ GROUP BY { groupItem [, groupItem ]* } ]
     [ HAVING booleanExpression ]

**Description**

Generally, HAVING and GROUP BY are used together. GROUP BY applies first for grouping and HAVING then applies for filtering.

**Precautions**

-  If the filtering condition is subject to the query results of GROUP BY, the HAVING clause, rather than the WHERE clause, must be used for filtering. HAVING and GROUP BY are used together. GROUP BY applies first for grouping and HAVING then applies for filtering.
-  Fields used in HAVING, except for those used for aggregate functions, must exist in GROUP BY.
-  The arithmetic operation and aggregate function are supported by the HAVING clause.

**Example**

Group the **transactions** according to **num**, use the HAVING clause to filter the records in which the maximum value derived from multiplying **price** with **amount** is higher than 5000, and return the filtered results.

::

   insert into temp SELECT num, max(price*amount) FROM transactions
     WHERE time > '2016-06-01'
     GROUP BY num
     HAVING max(price*amount)>5000;

UNION
-----

**Syntax**

::

   query UNION [ ALL ] query

**Description**

This statement is used to return the union set of multiple query results.

**Precautions**

-  Set operation is to join tables from head to tail under certain conditions. The quantity of columns returned by each SELECT statement must be the same. Column types must be the same. Column names can be different.
-  By default, the repeated records returned by UNION are removed. The repeated records returned by UNION ALL are not removed.

**Example**

Output the union set of Orders1 and Orders2 without duplicate records.

::

   insert into temp SELECT  * FROM Orders1
     UNION SELECT  * FROM Orders2;
