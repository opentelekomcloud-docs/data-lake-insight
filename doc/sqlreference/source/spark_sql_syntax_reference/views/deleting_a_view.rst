:original_name: dli_08_0131.html

.. _dli_08_0131:

Deleting a View
===============

Function
--------

This statement is used to delete views.

Syntax
------

::

   DROP VIEW [IF EXISTS] [db_name.]view_name;

Keywords
--------

DROP: Deletes the metadata of a specified view. Although views and tables have many common points, the DROP TABLE statement cannot be used to delete views.

Precautions
-----------

The to-be-deleted view must exist. If you run this statement to delete a view that does not exist, an error is reported. To avoid such an error, you can add **IF EXISTS** in this statement.

Example
-------

To delete a view named **student_view**, run the following statement:

::

   DROP VIEW student_view;
