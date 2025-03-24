:original_name: dli_08_15007.html

.. _dli_08_15007:

CREATE CATALOG
==============

Function
--------

This statement creates a catalog using specified attributes. If a catalog with the same name already exists, an exception is thrown.

Syntax
------

.. code-block::

   CREATE CATALOG catalog_name
     WITH (key1=val1, key2=val2, ...)

Description
-----------

**WITH OPTIONS**

Catalog attributes typically store additional information about the catalog.

The key and value of the **key1=val1** expression are string literals.
