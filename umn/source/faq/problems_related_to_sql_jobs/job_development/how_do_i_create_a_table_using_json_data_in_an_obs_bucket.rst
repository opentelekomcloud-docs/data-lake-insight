:original_name: dli_03_0108.html

.. _dli_03_0108:

How Do I Create a Table Using JSON Data in an OBS Bucket?
=========================================================

DLI allows you to associate JSON data in an OBS bucket to create tables in asynchronous mode.

The statement for creating the table is as follows:

.. code-block::

   create table tb1 using json options(path 'obs://....')
