:original_name: dli_08_15068.html

.. _dli_08_15068:

Set Operations
==============

Union/Union ALL/Intersect/Except
--------------------------------

**Syntax**

::

   query UNION [ ALL ] | Intersect | Except query

**Description**

-  UNION is used to return the union set of multiple query results.
-  INTERSECT is used to return the intersection of multiple query results.
-  EXCEPT is used to return the difference set of multiple query results.

**Precautions**

-  Set operations join tables from head to tail under certain conditions. The quantity of columns returned by each SELECT statement must be the same. Column types must be the same. Column names can be different.
-  By default, UNION takes only distinct records while UNION ALL does not remove duplicates from the result.

**Example**

Output distinct records found in either Orders1 and Orders2 tables.

::

   insert into temp SELECT  * FROM Orders1
     UNION SELECT  * FROM Orders2;

IN
--

**Syntax**

::

   SELECT [ ALL | DISTINCT ]   { * | projectItem [, projectItem ]* }
     FROM tableExpression
     WHERE column_name IN (value (, value)* ) | query

**Description**

The IN operator allows multiple values to be specified in the WHERE clause. It returns true if the expression exists in the given table subquery.

**Precautions**

The subquery table must consist of a single column, and the data type of the column must be the same as that of the expression.

**Example**

Return **user** and **amount** information of the products in **NewProducts** of the **Orders** table.

::

   insert into temp SELECT user, amount
   FROM Orders
   WHERE product IN (
       SELECT product FROM NewProducts
   );
