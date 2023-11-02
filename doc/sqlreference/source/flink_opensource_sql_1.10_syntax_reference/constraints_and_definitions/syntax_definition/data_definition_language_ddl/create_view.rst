:original_name: dli_08_0295.html

.. _dli_08_0295:

CREATE VIEW
===========

Syntax
------

.. code-block::

   CREATE VIEW [IF NOT EXISTS] view_name
     [{columnName [, columnName ]* }] [COMMENT view_comment]
     AS query_expression

Function
--------

Create a view with multiple layers nested in it to simplify the development process.

Description
-----------

**IF NOT EXISTS**

If the view already exists, nothing happens.

Example
-------

Create a view named **viewName**.

.. code-block::

   create view viewName as select * from dataSource
