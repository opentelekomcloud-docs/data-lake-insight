:original_name: dli_03_0214.html

.. _dli_03_0214:

Why Is the create_date Field in the RDS Table Is a Timestamp in the DLI query result?
=====================================================================================

Spark does not have the datetime type and uses the TIMESTAMP type instead.

You can use a function to convert data types.

The following is an example.

select cast(create_date as string), \* from table where create_date>'2221-12-01 00:00:00';
