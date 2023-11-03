:original_name: dli_08_0075.html

.. _dli_08_0075:

SQL Syntax Constraints and Definitions
======================================

Syntax Constraints
------------------

-  Currently, Flink SQL only supports the following operations: SELECT, FROM, WHERE, UNION, aggregation, window, JOIN between stream and table data, and JOIN between streams.
-  Data cannot be inserted into the source stream.
-  The sink stream cannot be used to perform query operations.

Data Types Supported by Syntax
------------------------------

-  Basic data types: VARCHAR, STRING, BOOLEAN, TINYINT, SMALLINT, INTEGER/INT, BIGINT, REAL/FLOAT, DOUBLE, DECIMAL, DATE, TIME, and TIMESTAMP

-  Array: Square brackets ([]) are used to quote fields. The following is an example:

   ::

      insert into temp select CARDINALITY(ARRAY[1,2,3]) FROM OrderA;

Syntax Definition
-----------------

::

   INSERT INTO stream_name query;
   query:
     values
     | {
         select
         | selectWithoutFrom
         | query UNION [ ALL ] query
       }

   orderItem:
     expression [ ASC | DESC ]

   select:
     SELECT
     { * | projectItem [, projectItem ]* }
     FROM tableExpression [ JOIN tableExpression ]
     [ WHERE booleanExpression ]
     [ GROUP BY { groupItem [, groupItem ]* } ]
     [ HAVING booleanExpression ]

   selectWithoutFrom:
     SELECT [ ALL | DISTINCT ]
     { * | projectItem [, projectItem ]* }

   projectItem:
     expression [ [ AS ] columnAlias ]
     | tableAlias . *

   tableExpression:
     tableReference

   tableReference:
     tablePrimary
     [ [ AS ] alias [ '(' columnAlias [, columnAlias ]* ')' ] ]

   tablePrimary:
     [ TABLE ] [ [ catalogName . ] schemaName . ] tableName
     | LATERAL TABLE '(' functionName '(' expression [, expression ]* ')' ')'
     | UNNEST '(' expression ')'

   values:
     VALUES expression [, expression ]*

   groupItem:
     expression
     | '(' ')'
     | '(' expression [, expression ]* ')'
     | CUBE '(' expression [, expression ]* ')'
     | ROLLUP '(' expression [, expression ]* ')'
     | GROUPING SETS '(' groupItem [, groupItem ]* ')'
