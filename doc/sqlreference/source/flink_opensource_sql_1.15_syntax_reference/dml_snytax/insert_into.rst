:original_name: dli_08_15107.html

.. _dli_08_15107:

INSERT INTO
===========

This section describes how to use the **INSERT INTO** statement to write job results to a sink table.

Writing Data to a Sink Table
----------------------------

-  **Syntax**

   ::

        INSERT INTO your_sink
        SELECT ... FROM your_source WHERE ...

-  **Example**

   In this example, two tables **my_source** and **my_sink** are defined, and the **INSERT INTO** statement is used to select data from the source table and insert the data to the sink table.

   ::

      -- Use the datagen connector to create the source table my_source.
      CREATE TABLE my_source (
        name VARCHAR,
        age BIGINT
      ) WITH (
        'connector' = 'datagen');

      -- Use the JDBC connector to create the sink table my_sink.
      CREATE TABLE my_sink (
        name VARCHAR,
        age BIGINT
      ) WITH (
        'connector' = 'jdbc',
        'url' = 'jdbc:mysql://xxx/your-database',
        'table-name' = 'your-table',
        'username' = 'your-username',
        'password' = 'your-password'
      );

      -- Run the INSERT INTO statement to select data from the my_source table and insert the data into the my_sink table.
      INSERT INTO my_sink
      SELECT name, age
      FROM my_source;

Writing Data to Multiple Sink Tables
------------------------------------

**EXECUTE STATEMENT SET BEGIN... END;** is a required statement for writing data to multiple sink tables. It is used to define multiple data insertion operations in the same job.

.. caution::

   **EXECUTE STATEMENT SET BEGIN... END;** is required when data is written to multiple sink tables.

-  **Syntax**

   ::

      EXECUTE STATEMENT SET BEGIN
      -- First DML statement
        INSERT INTO your_sink1
        SELECT ... FROM your_source WHERE ...;

      -- Second DML statement
        INSERT INTO your_sink2
        SELECT ... FROM your_source WHERE ...

      ...
      END;

-  **Example**

   In this example, the source table **datagen_source** and sink tables **print_sinkA** and **print_sinkB** are defined. **EXECUTE STATEMENT** is used to execute two **INSERT INTO** statements to write the converted data to two different sinks.

   ::

      -- Use the datagen connector to create the source table datagen_source.
      CREATE TABLE datagen_source (
        name VARCHAR,
        age BIGINT
      ) WITH (
        'connector' = 'datagen'
      );

      -- Use the print connector to create the result tables print_sinkA and print_sinkB.

      CREATE TABLE print_sinkA(
        name VARCHAR,
        age BIGINT
      ) WITH (
        'connector' = 'print'
      );

      CREATE TABLE print_sinkB(
        name VARCHAR,
        age BIGINT
      ) WITH (
        'connector' = 'print'
      );

      -- Use EXECUTE STATEMENT SET BEGIN to execute two INSERT INTO statements.
      -- The first INSERT INTO statement converts the data in the datagen_source table as needed and writes the converted data to print_sinkA.
      -- The second INSERT INTO statement converts data as needed and writes the converted data to print_sinkB.
      EXECUTE  STATEMENT SET BEGIN
      INSERT INTO print_sinkA
        SELECT UPPER(name), min(age)
        FROM datagen_source
        GROUP BY UPPER(name);
      INSERT INTO print_sinkB
        SELECT LOWER(name), max(age)
        FROM datagen_source
        GROUP BY LOWER(name);
      END;
