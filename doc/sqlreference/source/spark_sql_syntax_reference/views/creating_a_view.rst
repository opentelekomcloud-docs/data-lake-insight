:original_name: dli_08_0130.html

.. _dli_08_0130:

Creating a View
===============

Function
--------

This statement is used to create views.

Syntax
------

::

   CREATE [OR REPLACE] VIEW view_name AS select_statement;

Keywords
--------

-  CREATE VIEW: creates views based on the given select statement. The result of the select statement will not be written into the disk.
-  OR REPLACE: updates views using the select statement. No error is reported and the view definition is updated using the SELECT statement if a view exists.

Precautions
-----------

-  The view to be created must not exist in the current database. Otherwise, an error will be reported. When the view exists, you can add keyword **OR REPLACE** to avoid the error message.

-  The table or view information contained in the view cannot be modified. If the table or view information is modified, the query may fail.

-  If the compute engines used for creating tables and views are different, the view query may fail due to incompatible varchar types.

   For example, if a table is created using Spark 3.\ *x*, you are advised to use Spark 2.\ *x* to create a view.

Example
-------

To create a view named **student_view** for the queried ID and name of the **student** table, run the following statement:

::

   CREATE VIEW student_view AS SELECT id, name FROM student;
