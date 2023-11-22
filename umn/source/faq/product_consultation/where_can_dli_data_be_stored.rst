:original_name: dli_03_0029.html

.. _dli_03_0029:

Where Can DLI Data Be Stored?
=============================

DLI data can be stored in either of the following:

-  OBS: Data used by SQL jobs, Spark jobs, and Flink jobs can be stored in OBS, reducing storage costs.
-  DLI: The column-based **Parquet** format is used in DLI. That is, the data is stored in the **Parquet** format. The storage cost is relatively high.
-  Datasource connection jobs can be stored in the connected services. Currently, CloudTable, CSS, DCS, DDS, GaussDB(DWS), MRS, and RDS are supported.
