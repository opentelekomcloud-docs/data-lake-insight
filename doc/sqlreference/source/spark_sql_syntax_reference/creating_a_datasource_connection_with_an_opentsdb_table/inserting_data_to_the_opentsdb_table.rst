:original_name: dli_08_0123.html

.. _dli_08_0123:

Inserting Data to the OpenTSDB Table
====================================

Function
--------

Run the **INSERT INTO** statement to insert the data in the DLI table to the associated **OpenTSDB metric**.

.. note::

   If no metric exists on the OpenTSDB, a new metric is automatically created on the OpenTSDB when data is inserted.

Syntax
------

::

   INSERT INTO TABLE TABLE_NAME SELECT * FROM DLI_TABLE;

::

   INSERT INTO TABLE TABLE_NAME VALUES(XXX);

Keywords
--------

.. table:: **Table 1** INSERT INTO keywords

   ========== ======================================
   Parameter  Description
   ========== ======================================
   TABLE_NAME Name of the associated OpenTSDB table.
   DLI_TABLE  Name of the DLI table created.
   ========== ======================================

Precautions
-----------

-  The inserted data cannot be **null**. If the inserted data is the same as the original data or only the **value** is different, the inserted data overwrites the original data.
-  **INSERT OVERWRITE** is not supported.
-  You are advised not to concurrently insert data into a table. If you concurrently insert data into a table, there is a possibility that conflicts occur, leading to failed data insertion.
-  The **TIMESTAMP** format supports only yyyy-MM-dd hh:mm:ss.

Example
-------

::

   INSERT INTO TABLE opentsdb_table VALUES('xxx','xxx','2018-05-03 00:00:00',21);
