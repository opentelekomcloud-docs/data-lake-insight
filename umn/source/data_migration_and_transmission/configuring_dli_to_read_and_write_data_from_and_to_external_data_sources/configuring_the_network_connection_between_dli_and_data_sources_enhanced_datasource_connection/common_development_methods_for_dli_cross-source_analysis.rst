:original_name: dli_01_0410.html

.. _dli_01_0410:

Common Development Methods for DLI Cross-Source Analysis
========================================================

Cross-Source Analysis
---------------------

If DLI needs to access external data sources, you need to establish enhanced datasource connections to enable the network between DLI and the data sources, and then develop different types of jobs to access the data sources. This is the process of DLI cross-source analysis.

This section describes how to develop data sources supported by DLI for cross-source analysis.

Notes
-----

-  Flink jobs can directly access DIS, OBS, and SMN data sources without using datasource connections.
-  You are advised to use enhanced datasource connections to connect DLI to data sources.

DLI Supported Data Sources
--------------------------

:ref:`Table 1 <dli_01_0410__table1771918377534>` lists the data sources supported by DLI. For how to use the data sources, see the *Data Lake Insight SQL Syntax Reference*.

.. _dli_01_0410__table1771918377534:

.. table:: **Table 1** Supported data sources

   ============= ======= ============= ============= =============
   Service       SQL Job Spark Jar Job Flink SQL Job Flink Jar Job
   ============= ======= ============= ============= =============
   APIG          x       x             Y             x
   CSS           Y       Y             Y             Y
   DCS Redis     Y       Y             Y             Y
   DDS           Y       Y             Y             Y
   DMS Kafka     x       x             Y             Y
   GaussDB(DWS)  Y       Y             Y             Y
   MRS HBase     Y       Y             Y             Y
   MRS Kafka     x       x             Y             Y
   MRS OpenTSDB  Y       Y             x             Y
   RDS for MySQL Y       Y             Y             Y
   RDS PostGre   Y       Y             Y             Y
   ============= ======= ============= ============= =============
