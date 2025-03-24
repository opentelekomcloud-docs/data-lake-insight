:original_name: dli_08_15009.html

.. _dli_08_15009:

CREATE VIEW
===========

Function
--------

This statement creates a view based on the given query statement. If a view with the same name already exists in the database, an exception is thrown.

Syntax
------

.. code-block::

   CREATE [TEMPORARY] VIEW [IF NOT EXISTS] [catalog_name.][db_name.]view_name
     [{columnName [, columnName ]* }] [COMMENT view_comment]
     AS query_expression

Description
-----------

**TEMPORARY**

Create a temporary view with catalogs and database namespaces and overwrite the original view.

**IF NOT EXISTS**

If the view already exists, nothing happens.

Example
-------

Create a view named **viewName**.

.. code-block::

   create view viewName as select * from dataSource
