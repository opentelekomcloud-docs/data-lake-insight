:original_name: dli_01_0228.html

.. _dli_01_0228:

Understanding Data Catalogs, Databases, and Tables
==================================================

Databases and tables are the basis for developing SQL and Spark jobs. Before running a job, you need to define databases and tables based on your service scenarios.

.. note::

   Flink allows for dynamic data types, enabling the definition of data structures at runtime without the need for predefined metadata.

Data Catalog
------------

A data catalog is a metadata management object that can contain multiple databases.

For how to create a database and a table in a DLI data catalog, see :ref:`Creating a Database and Table on the DLI Console <dli_01_0005>`.

Database
--------

A database is a repository of data organized, stored, and managed on computer storage devices according to data structures. Databases are typically used to store, retrieve, and manage structured data, consisting of multiple data tables that are interrelated through keys and indexes.

Table
-----

Tables are one of the most important components of a database, consisting of rows and columns. Each row represents a data item, while each column represents a property or feature of the data. Tables are used to organize and store specific types of data, making it possible to query and analyze the data effectively.

A database is a framework, and tables are its essential content. A database contains one or more tables.

You can create databases and tables on the management console or using SQL statements. This section describes how to create a database and a table on the management console.

.. note::

   When creating a database or table, you need to grant permissions to other users so that they can view the new database or table.

Table Metadata
--------------

Metadata is data used to define the type of data. It primarily describes information about the data itself, including its source, size, format, or other characteristics. In database fields, metadata is used to interpret the content of a data warehouse.

When creating a table, metadata is defined by three columns: column name, type, and column description.

Table Types Supported by DLI
----------------------------

-  **DLI table**

   DLI tables are data tables stored in a DLI data lake. They can store structured, semi-structured, and unstructured data.

   Data in DLI tables is stored internally within the DLI service, resulting in better query performance. This makes it suitable for time-sensitive services, such as interactive queries.

   In the navigation pane on the left of the DLI console, choose **Data Management** > **Databases and Tables**. On the displayed page, click the name of a database. In the displayed table list, tables whose **Type** is **MANAGED** are DLI tables.

-  **OBS table**

   Data in OBS tables is stored in the OBS service, which is suitable for latency-insensitive services, such as historical data statistics and analysis.

   An OBS table stores data in the form of objects. Each object contains data and related metadata.

   In the navigation pane on the left of the DLI console, choose **Data Management** > **Databases and Tables**. On the displayed page, click the name of a database. In the displayed table list, tables whose **Type** is **EXTERNAL** and **Storage Location** is **OBS** are OBS tables.

-  **View table**

   A view table is a virtual table that does not store actual data. Instead, it dynamically generates data based on the defined query logic. Views are typically used to simplify complex queries or provide customized data views for different users or applications.

   A view table can be created based on one or multiple tables, providing a flexible way to display data without affecting the storage and organization of the underlying data.

   In the navigation pane on the left of the DLI console, choose **Data Management** > **Databases and Tables**. On the displayed page, click the name of a database. In the displayed table list, tables whose **Type** is **VIEW** are view tables.

   .. note::

      A view can only be created using SQL statements. You cannot create a view on the **Create Table** page. Table or view information in a view cannot be modified. Otherwise, the query may fail.

-  **Datasource table**

   A datasource table is a data table that can be queried and analyzed across multiple data sources. This type of tables can integrate data from varying data sources and provide a unified data view.

   Datasource tables are typically used in data warehouse and data lake architectures, allowing users to perform complex queries across multiple data sources.

   In the navigation pane on the left of the DLI console, choose **Data Management** > **Databases and Tables**. On the displayed page, click the name of a database. In the displayed table list, tables whose **Type** is **EXTERNAL** and **Storage Location** is not **OBS** are datasource tables.

Notes and Constraints on Databases and Tables
---------------------------------------------

.. table:: **Table 1** Notes and constraints on DLI resources

   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Item                              | Description                                                                                                                                                                                                                                                                                                   |
   +===================================+===============================================================================================================================================================================================================================================================================================================+
   | Database                          | -  **default** is the database built in DLI. You cannot create a database named **default**.                                                                                                                                                                                                                  |
   |                                   | -  DLI supports a maximum of 50 databases.                                                                                                                                                                                                                                                                    |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Data table                        | -  DLI supports a maximum of 5,000 tables.                                                                                                                                                                                                                                                                    |
   |                                   | -  DLI supports the following table types:                                                                                                                                                                                                                                                                    |
   |                                   |                                                                                                                                                                                                                                                                                                               |
   |                                   |    -  **MANAGED**: Data is stored in a DLI table.                                                                                                                                                                                                                                                             |
   |                                   |    -  **EXTERNAL**: Data is stored in an OBS table.                                                                                                                                                                                                                                                           |
   |                                   |    -  **View**: A view can only be created using SQL statements.                                                                                                                                                                                                                                              |
   |                                   |    -  Datasource table: The table type is also **EXTERNAL**.                                                                                                                                                                                                                                                  |
   |                                   |                                                                                                                                                                                                                                                                                                               |
   |                                   | -  You cannot specify a storage path when creating a DLI table.                                                                                                                                                                                                                                               |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Data import                       | -  Only OBS data can be imported to DLI or OBS.                                                                                                                                                                                                                                                               |
   |                                   | -  You can import data in CSV, Parquet, ORC, JSON, or Avro format from OBS to tables created on DLI.                                                                                                                                                                                                          |
   |                                   | -  To import data in CSV format to a partitioned table, place the partition column in the last column of the data source.                                                                                                                                                                                     |
   |                                   | -  The encoding format of imported data can only be UTF-8.                                                                                                                                                                                                                                                    |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Data export                       | -  Data in DLI tables (whose table type is **MANAGED**) can only be exported to OBS buckets, and the export path must contain a folder.                                                                                                                                                                       |
   |                                   | -  The exported file is in JSON format, and the text format can only be UTF-8.                                                                                                                                                                                                                                |
   |                                   | -  Data can be exported across accounts. That is, after account B authorizes account A, account A has the permission to read the metadata and permission information of account B's OBS bucket as well as the read and write permissions on the path. Account A can export data to the OBS path of account B. |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Table Management Page
---------------------

From the **Data Management** page, click the database name or **Tables** in the **Operation** column to switch to the table management page.

The displayed page lists all tables created in the current database. You can view the table type, data storage location, and other information. Tables are listed in chronological order by default, with the most recently created tables displayed at the top.
