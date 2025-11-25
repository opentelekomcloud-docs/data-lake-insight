:original_name: dli_07_0004.html

.. _dli_07_0004:

Related Services
================

OBS
---

OBS works as the data source and data storage system for DLI, and delivers the following capabilities:

-  Data source: DLI provides an API for you to import data from corresponding OBS paths to DLI tables.

   For details about the API, see **Resource-related APIs** > **Table-related APIs** > **Importing Data** in the *Data Lake Insight API Reference*.

-  Data storage: You can create OBS tables on DLI. However, such tables only store metadata while data content is stored in corresponding OBS paths.

   For details, see **Creating an OBS Table Using the DataSource Syntax** and **Creating an OBS Table Using the Hive Syntax** in the *Data Lake Insight SQL Syntax Reference*.

-  Data backup: DLI provides an API for you to export the data in DLI to OBS for backup.

   For details about the API, see **Resource-related APIs** > **Table-related APIs** > **Exporting Data** in the *Data Lake Insight API Reference*.

-  Query result storage: DLI provides an API for you to save routine query result data on OBS.

   For details about the API, see "SQL Job related APIs" > "Exporting Query Results" in *Data Lake Insight API Reference*.

IAM
---

Identity and Access Management (IAM) authenticates access to DLI.

For details, see :ref:`Creating an IAM User and Granting Permissions <dli_01_0418>` and :ref:`Using IAM Roles or Policies to Grant Access to DLI <dli_01_0451>`.

CTS
---

Cloud Trace Service (CTS) audits performed DLI operations.

For details about DLI operations that can be recorded by CTS, see :ref:`DLI Operations That Can Be Recorded by CTS <dli_01_0318>`.

Cloud Eye
---------

Cloud Eye helps monitor job metrics for DLI, delivering status information in a concise and efficient manner.

For details, see :ref:`Monitoring DLI Using Cloud Eye <dli_01_0445>`.

SMN
---

Simple Message Notification (SMN) can send notifications to users when a job running exception occurs on DLI.

For details, see :ref:`Creating an SMN Topic <dli_01_0421>`.

DIS
---

Data Ingestion Service (DIS) imports data to DLI through streams.

For details, see :ref:`Managing Enhanced Datasource Connections <dli_01_0509>`.

RDS
---

Relational Database Service (RDS) works as the data source and data storage system for DLI, and delivers the following capabilities:

-  Data source: DLI allows you to import RDS data using DataFrame or SQL.
-  Query result storage: DLI uses the SQL INSERT syntax to store query result data to RDS tables.

For how to access RDS data through a DLI datasource connection, see :ref:`Common Development Methods for DLI Cross-Source Analysis <dli_01_0410>`.

GaussDB(DWS)
------------

GaussDB(DWS) works as the data source and data storage system for DLI, and delivers the following capabilities:

-  Data source: DLI allows you to import GaussDB(DWS) data using DataFrame or SQL.
-  Query result storage: DLI uses the SQL INSERT syntax to store query result data to GaussDB(DWS) tables.

For how to access GaussDB(DWS) data through a DLI datasource connection, see :ref:`Common Development Methods for DLI Cross-Source Analysis <dli_01_0410>`.

CSS
---

CSS works as the data source and data storage system for DLI, and delivers the following capabilities:

-  Data source: DLI allows you to import CSS data using DataFrame or SQL.
-  Query result storage: DLI uses the SQL INSERT syntax to store query result data to CSS tables.

For how to access CSS data through a DLI datasource connection, see :ref:`Common Development Methods for DLI Cross-Source Analysis <dli_01_0410>`.

DCS
---

Distributed Cache Service (DCS) works as the data source and data storage system for DLI, and delivers the following capabilities:

-  Data source: DLI allows you to import DCS data using DataFrame or SQL.
-  Query result storage: DLI uses the SQL INSERT syntax to store query result data to DCS tables.

For how to access DCS data through a DLI datasource connection, see :ref:`Common Development Methods for DLI Cross-Source Analysis <dli_01_0410>`.

DDS
---

Document Database Service (DDS) works as the data source and data storage system for DLI, and delivers the following capabilities:

-  Data source: DLI allows you to import DDS data using DataFrame or SQL.
-  Query result storage: DLI uses the SQL INSERT syntax to store query result data to DDS tables.

For how to access DDS data through a DLI datasource connection, see :ref:`Common Development Methods for DLI Cross-Source Analysis <dli_01_0410>`.

MRS
---

MapReduce Service (MRS) works as the data source and data storage system for DLI, and delivers the following capabilities:

-  Data source: DLI allows you to import MRS data using DataFrame or SQL.
-  Query result storage: DLI uses the SQL INSERT syntax to store query result data to MRS tables.

For how to access MRS data through a DLI datasource connection, see :ref:`Common Development Methods for DLI Cross-Source Analysis <dli_01_0410>`.

DataArts Studio
---------------

DataArts Studio is a one-stop big data development platform for collaborations and provides fully-managed big data scheduling capabilities. It manages various big data services, making big data more accessible than ever before and helping you effortlessly build big data processing centers.

-  The DLI SQL node in DataArts Studio transfers SQL statements to DLI for execution.
-  The DLI Spark node in DataArts Studio is used to execute a predefined Spark job.

For details about how to submit DLI jobs using DataArts Studio, see *DataArts Studio User Guide*.
