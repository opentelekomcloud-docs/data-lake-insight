:original_name: dli_03_0108.html

.. _dli_03_0108:

How Do I Create a Table Using JSON Data in an OBS Bucket?
=========================================================

To associate JSON data nested in an OBS bucket, you can create a table in asynchronous mode.

The following is an example of a table creation statement that shows how to use JSON format options to specify the path in OBS:

.. code-block::

   create table tb1 using json options(path 'obs://....')

-  **using json**: JSON format is used.
-  **options**: used to set table options.
-  **path**: path of the JSON file in OBS.
