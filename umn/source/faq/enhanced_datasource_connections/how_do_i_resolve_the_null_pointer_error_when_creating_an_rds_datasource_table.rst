:original_name: dli_03_0250.html

.. _dli_03_0250:

How Do I Resolve the Null Pointer Error When Creating an RDS Datasource Table?
==============================================================================

Symptom
-------

The system failed to create a datasource RDS table, and null pointer error was reported.

Cause Analysis
--------------

The following table creation statement was used:

.. code-block::

   CREATE TABLE IF NOT EXISTS dli_to_rds
    USING JDBC OPTIONS (
    'url'='jdbc:mysql://to-rds-1174405119-oLRHAGE7.datasource.com:5432/postgreDB',
    'driver'='org.postgresql.Driver',
    'dbtable'='pg_schema.test1',
    'passwdauth' = 'xxx',
    'encryption' = 'true');

The RDS database is in a PostGre cluster, and the protocol header in the URL is invalid.

Procedure
---------

Change the URL to **url'='jdbc:postgresql://to-rds-1174405119-oLRHAGE7.datasource.com:5432/postgreDB** and run the creation statement. The datasource table is successfully created.
