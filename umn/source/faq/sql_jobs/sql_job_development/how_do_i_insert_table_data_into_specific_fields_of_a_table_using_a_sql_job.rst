:original_name: dli_03_0191.html

.. _dli_03_0191:

How Do I Insert Table Data into Specific Fields of a Table Using a SQL Job?
===========================================================================

If you need to insert data into a table but only want to specify certain fields, you can use the **INSERT INTO** statement combined with the **SELECT** clause.

However, DLI currently does not support inserting data into partial columns directly in the **INSERT INTO** statement. You need to ensure that the number and types of fields selected in the **SELECT** clause match the Schema information of the target table. That is, ensure that the data types and column field numbers of the source table and sink table are the same to avoid insertion failure.

If some fields in the sink table are not specified in the **SELECT** clause, these fields may also be inserted with default values or set to null values (depending on whether the field allows null values).
