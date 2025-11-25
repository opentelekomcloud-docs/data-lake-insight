:original_name: dli_03_0278.html

.. _dli_03_0278:

How Do I Resolve the "Input argument to rand must be a constant" Error in Spark 3.3.1 SQL Statements?
=====================================================================================================

Symptom
-------

When the rand function is used in SQL statements of Spark 3.3.1, the error message "Input argument to rand must be a constant" is displayed.

Solution
--------

In Spark 3.3.1, the rand function requires constant parameters.

To fix this error, update the parameters in the rand function within the SQL statements to constant parameters and rerun the SQL statements.
