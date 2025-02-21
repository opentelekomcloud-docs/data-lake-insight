:original_name: dli_03_0029.html

.. _dli_03_0029:

Where Can Data Be Stored in DLI?
================================

What Data Formats Can Be Stored in DLI?
---------------------------------------

Supported data formats:

-  Parquet
-  CSV
-  ORC
-  JSON
-  Avro


Where Can Data Be Stored in DLI?
--------------------------------

-  OBS: Data used by SQL jobs, Spark jobs, and Flink jobs can be stored in OBS, reducing storage costs.
-  DLI: The column-based **Parquet** format is used in DLI. That is, the data is stored in the **Parquet** format. The storage cost is relatively high.
-  Datasource connection jobs can be stored in the connected services. Currently, CloudTable, CSS, DCS, DDS, GaussDB(DWS), MRS, and RDS are supported.

What Are the Differences Between DLI Tables and OBS Tables?
-----------------------------------------------------------

-  DLI tables store data within the DLI service, keeping you unaware of the storage path.
-  OBS tables store data in your OBS buckets, allowing you to manage source data files.

DLI tables offer advanced features like permission control and caching acceleration, providing better performance than foreign tables. However, they come with storage fees.
