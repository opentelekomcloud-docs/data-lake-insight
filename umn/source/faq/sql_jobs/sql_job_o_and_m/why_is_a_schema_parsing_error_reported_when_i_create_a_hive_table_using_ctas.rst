:original_name: dli_03_0046.html

.. _dli_03_0046:

Why Is a Schema Parsing Error Reported When I Create a Hive Table Using CTAS?
=============================================================================

Currently, DLI supports the creation of TEXTFILE, SEQUENCEFILE, RCFILE, ORC, AVRO, and PARQUET tables using the Hive syntax.

If you create a table using CTAS statements, specify the AVRO format, and then use a number as input for the SELECT query statement (for example, **CREATE TABLE tb_avro STORED AS AVRO AS SELECT 1**), a schema parsing exception will be reported.

If the column name is not specified, the content after SELECT is used as both the column name and inserted value. The column name of the AVRO table cannot be a digit. Otherwise, an error will be reported, indicating that the schema fails to be parsed.

**Solution**: You can use **CREATE TABLE tb_avro STORED AS AVRO AS SELECT 1 AS colName** to specify the column name or set the storage format to a format other than AVRO.
