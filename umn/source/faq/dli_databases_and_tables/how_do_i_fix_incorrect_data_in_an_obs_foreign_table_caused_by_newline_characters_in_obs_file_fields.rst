:original_name: dli_03_0181.html

.. _dli_03_0181:

How Do I Fix Incorrect Data in an OBS Foreign Table Caused by Newline Characters in OBS File Fields?
====================================================================================================

Symptom
-------

When an OBS foreign table is created, a field in the specified OBS file contains a carriage return line feed (CRLF) character. As a result, the data is incorrect.

The statement for creating an OBS foreign table is similar to the following:

.. code-block::

   CREATE TABLE test06 (name string, id int, no string) USING csv OPTIONS (path "obs://dli-test-001/test.csv");

The file contains the following information (example):

.. code-block::

   Jordon,88,"aa
   bb"

A carriage return exists between **aa** and **bb** in the last field. As a result, he data in the **test06** table is displayed as follows:

.. code-block::

   name  id  classno
   Jordon  88  aa
   bb" null    null

Solution
--------

When creating an OBS foreign table, set **multiLine** to **true** to specify that the column data contains CRLF characters. The following is an example to solve the problem:

.. code-block::

   CREATE TABLE test06 (name string, id int, no string) USING csv OPTIONS (path "obs://dli-test-001/test.csv",multiLine=true);
