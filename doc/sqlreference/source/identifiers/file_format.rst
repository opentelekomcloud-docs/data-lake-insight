:original_name: dli_08_0020.html

.. _dli_08_0020:

file_format
===========

Format
------

\| AVRO

\| CSV

\| JSON

\| ORC

\| PARQUET

Description
-----------

-  Currently, the preceding formats are supported.
-  Both **USING** and **STORED AS** can be used for specifying the data format. You can specify the preceding data formats by **USING**, but only the **ORC** and **PARQUET** formats by **STORED AS**.
-  **ORC** has optimized **RCFile** to provide an efficient method to store **Hive** data.
-  **PARQUET** is an analytical service-oriented and column-based storage format.
