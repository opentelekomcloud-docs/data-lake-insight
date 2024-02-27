:original_name: dli_08_0378.html

.. _dli_08_0378:

Data Manipulation Language (DML)
================================

DML Statements
--------------

**Syntax**

.. code-block::

   INSERT INTO table_name [PARTITION part_spec] query

   part_spec:  (part_col_name1=val1 [, part_col_name2=val2, ...])

   query:
     values
     | {
         select
         | selectWithoutFrom
         | query UNION [ ALL ] query
         | query EXCEPT query
         | query INTERSECT query
       }
       [ ORDER BY orderItem [, orderItem ]* ]
       [ LIMIT { count | ALL } ]
       [ OFFSET start { ROW | ROWS } ]
       [ FETCH { FIRST | NEXT } [ count ] { ROW | ROWS } ONLY]

   orderItem:
     expression [ ASC | DESC ]

   select:
     SELECT [ ALL | DISTINCT ]
     { * | projectItem [, projectItem ]* }
     FROM tableExpression
     [ WHERE booleanExpression ]
     [ GROUP BY { groupItem [, groupItem ]* } ]
     [ HAVING booleanExpression ]
     [ WINDOW windowName AS windowSpec [, windowName AS windowSpec ]* ]

   selectWithoutFrom:
     SELECT [ ALL | DISTINCT ]
     { * | projectItem [, projectItem ]* }

   projectItem:
     expression [ [ AS ] columnAlias ]
     | tableAlias . *

   tableExpression:
     tableReference [, tableReference ]*
     | tableExpression [ NATURAL ] [ LEFT | RIGHT | FULL ] JOIN tableExpression [ joinCondition ]

   joinCondition:
     ON booleanExpression
     | USING '(' column [, column ]* ')'

   tableReference:
     tablePrimary
     [ matchRecognize ]
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

   windowRef:
       windowName
     | windowSpec

   windowSpec:
       [ windowName ]
       '('
       [ ORDER BY orderItem [, orderItem ]* ]
       [ PARTITION BY expression [, expression ]* ]
       [
           RANGE numericOrIntervalExpression {PRECEDING}
         | ROWS numericExpression {PRECEDING}
       ]
       ')'

   matchRecognize:
         MATCH_RECOGNIZE '('
         [ PARTITION BY expression [, expression ]* ]
         [ ORDER BY orderItem [, orderItem ]* ]
         [ MEASURES measureColumn [, measureColumn ]* ]
         [ ONE ROW PER MATCH ]
         [ AFTER MATCH
               ( SKIP TO NEXT ROW
               | SKIP PAST LAST ROW
               | SKIP TO FIRST variable
               | SKIP TO LAST variable
               | SKIP TO variable )
         ]
         PATTERN '(' pattern ')'
         [ WITHIN intervalLiteral ]
         DEFINE variable AS condition [, variable AS condition ]*
         ')'

   measureColumn:
         expression AS alias

   pattern:
         patternTerm [ '|' patternTerm ]*

   patternTerm:
         patternFactor [ patternFactor ]*

   patternFactor:
         variable [ patternQuantifier ]

   patternQuantifier:
         '*'
     |   '*?'
     |   '+'
     |   '+?'
     |   '?'
     |   '??'
     |   '{' { [ minRepeat ], [ maxRepeat ] } '}' ['?']
     |   '{' repeat '}'

**Precautions**

Flink SQL uses a lexical policy for identifier (table, attribute, function names) similar to Java:

-  The case of identifiers is preserved whether they are quoted.
-  Identifiers are matched case-sensitively.
-  Unlike Java, back-ticks allow identifiers to contain non-alphanumeric characters (for example, **SELECT a AS \`my field\` FROM t**).

String literals must be enclosed in single quotes (for example, **SELECT'Hello World'**). Duplicate a single quote for escaping (for example, **SELECT 'It''s me.'**). Unicode characters are supported in string literals. If explicit Unicode points are required, use the following syntax:

-  Use the backslash (\\) as an escaping character (default): **SELECT U&'\\263A'**
-  Use a custom escaping character: **SELECT U&'#263A' UESCAPE '#'**
