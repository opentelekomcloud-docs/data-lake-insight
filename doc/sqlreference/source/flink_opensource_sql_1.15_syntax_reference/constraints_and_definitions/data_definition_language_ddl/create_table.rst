:original_name: dli_08_15006.html

.. _dli_08_15006:

CREATE TABLE
============

Function
--------

This statement creates a table using the specified table name. However, if a table with the same name already exists in the catalog, the registration process will fail.

Syntax
------

.. code-block::

   CREATE TABLE [IF NOT EXISTS] [catalog_name.][db_name.]table_name
     (
       { <physical_column_definition> | <metadata_column_definition> | <computed_column_definition> }[ , ...n]
       [ <watermark_definition> ]
       [ <table_constraint> ][ , ...n]
     )
     [COMMENT table_comment]
     [PARTITIONED BY (partition_column_name1, partition_column_name2, ...)]
     WITH (key1=val1, key2=val2, ...)
     [ LIKE source_table [( <like_options> )] ]

   <physical_column_definition>:
     column_name column_type [ <column_constraint> ] [COMMENT column_comment]

   <column_constraint>:
     [CONSTRAINT constraint_name] PRIMARY KEY NOT ENFORCED

   <table_constraint>:
     [CONSTRAINT constraint_name] PRIMARY KEY (column_name, ...) NOT ENFORCED

   <metadata_column_definition>:
     column_name column_type METADATA [ FROM metadata_key ] [ VIRTUAL ]

   <computed_column_definition>:
     column_name AS computed_column_expression [COMMENT column_comment]

   <watermark_definition>:
     WATERMARK FOR rowtime_column_name AS watermark_strategy_expression

   <source_table>:
     [catalog_name.][db_name.]table_name

   <like_options>:
   {
      { INCLUDING | EXCLUDING } { ALL | CONSTRAINTS | PARTITIONS }
    | { INCLUDING | EXCLUDING | OVERWRITING } { GENERATED | OPTIONS | WATERMARKS }
   }[, ...]

Description
-----------

**COMPUTED COLUMN**

A computed column is a virtual column generated using **column_name AS computed_column_expression**. A computed column evaluates an expression that can reference other columns declared in the same table. The column itself is not physically stored within the table. A computed column could be defined using **cost AS price \* quantity**. This expression can contain any combination of physical columns, constants, functions, or variables, but cannot contain any subquery.

In Flink, a computed column is used to define the time attribute in **CREATE TABLE** statements. A processing time attribute can be defined easily via **proc AS PROCTIME()** using the system's **PROCTIME()** function. The event time column may be obtained from an existing field. In this case, you can use the computed column to obtain event time. For example, if the original field is not of the **TIMESTAMP(3)** type or is nested in a JSON string, you can use computed columns.

Note:

-  An expression that defines a computed column in a source table is calculated after data is read from the data source. The column can be used in the **SELECT** statement.
-  A computed column cannot be the target of an **INSERT** statement. In an **INSERT** statement, the schema of the **SELECT** statement must be the same as that of the target table that does not have a computed column.

**WATERMARK**

The **WATERMARK** clause defines the event time attribute of a table and takes the form **WATERMARK FOR rowtime_column_name AS watermark_strategy_expression**.

**rowtime_column_name** defines an existing column that is marked as the event time attribute of the table. The column must be of the **TIMESTAMP(3)** type and must be the top-level column in the schema. It can also be a computed column.

**watermark_strategy_expression** defines the watermark generation strategy. It allows arbitrary non-query expressions, including computed columns, to calculate the watermark. The expression return type must be **TIMESTAMP(3)**, which represents the timestamp since the Epoch. The returned watermark will be emitted only if it is non-null and its value is greater than the previously emitted local watermark (to preserve the contract of ascending watermarks). The watermark generation expression is evaluated by the framework for every record. The framework will periodically emit the largest generated watermark. If the current watermark is still identical to the previous one, or is null, or the value of the returned watermark is smaller than that of the last emitted one, then no new watermark will be emitted. A watermark is emitted in an interval defined by **pipeline.auto-watermark-interval**. If the watermark interval is 0 ms, a watermark will be emitted per record if it is not null and greater than the last emitted one.

When using event time semantics, tables must contain an event time attribute and watermark strategy.

Flink provides several commonly used watermark strategies.

-  Strictly ascending timestamps: **WATERMARK FOR rowtime_column AS rowtime_column**

   Emits a watermark of the maximum observed timestamp so far. Rows that have a timestamp bigger than the maximum timestamp are not late.

-  Ascending timestamps: **WATERMARK FOR rowtime_column AS rowtime_column - INTERVAL '0.001' SECOND**

   Emits a watermark of the maximum observed timestamp so far minus 1. Rows that have a timestamp bigger than or equal to the maximum timestamp are not late.

-  Bounded out-of-order timestamps: **WATERMARK FOR rowtime_column AS rowtime_column - INTERVAL 'string' timeUnit**

   Emits a watermark, which is the maximum observed timestamp minus the specified delay, for example, **WATERMARK FOR rowtime_column AS rowtime_column - INTERVAL '5' SECOND** is a 5-second delayed watermark strategy.

   .. code-block::

      CREATE TABLE Orders (
          user BIGINT,
          product STRING,
          order_time TIMESTAMP(3),
          WATERMARK FOR order_time AS order_time - INTERVAL '5' SECOND
      ) WITH ( . . . );

**PRIMARY KEY**

The primary key constraint is a hint for Flink to leverage for optimizations. It tells that a column or a set of columns of a table or a view are unique and they do not contain null. Neither of columns in a primary can be nullable. The primary key therefore uniquely identifies a row in a table.

The primary key constraint can be either declared along with a column definition (a column constraint) or as a single line (a table constraint). For both cases, it should only be declared as a singleton. If you define multiple primary key constraints at the same time, an exception would be thrown.

Validity Check

SQL standard specifies that a constraint can either be **ENFORCED** or **NOT ENFORCED**. This controls if the constraint checks are performed on the incoming/outgoing data. Flink does not own the data and therefore the only mode we want to support is the **NOT ENFORCED** mode. It is up to the user to ensure that the query enforces key integrity.

Flink will assume correctness of the primary key by assuming that the columns nullability is aligned with the columns in the primary key. Connectors should ensure those are aligned.

Note: In a **CREATE TABLE** statement, creating a primary key constraint will alter the columns nullability, which means, a column with a primary key constraint is not nullable.

**PARTITIONED BY**

Partition the created table by the specified columns. A directory is created for each partition if this table is used as a file system sink.

**WITH OPTIONS**

Table properties used to create a table source/sink. The properties are usually used to find and create the underlying connector.

The key and value of expression **key1=val1** should both be string literal.

Note: The table name can be in any of the following formats: 1. catalog_name.db_name.table_name 2. db_name.table_name 3. table_name. Tables named in the **catalog_name.db_name.table_name** format are registered with metastore along with the catalog named **catalog_name** and the database named **db_name**. Tables named in the **uses db_name.table_name** format will be registered with the current table environment's catalog and the database will be named **db_name**. Tables named in the **table_name** format will be registered with the running catalog and database.

Note: Tables registered using the **CREATE TABLE** statement can be used as both the table source and table sink. We cannot decide if it is used as a source or sink until it is referenced in the DMLs.
