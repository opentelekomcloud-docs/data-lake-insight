:original_name: dli_01_0228.html

.. _dli_01_0228:

Overview
========

DLI database and table management provide the following functions:

-  :ref:`Database Permission Management <dli_01_0447>`
-  :ref:`Table Permission Management <dli_01_0448>`
-  :ref:`Creating a Database or a Table <dli_01_0005>`
-  :ref:`Deleting a Database or a Table <dli_01_0011>`
-  :ref:`Changing the Owners of Databases and Tables <dli_01_0376>`
-  :ref:`Importing Data <dli_01_0253>`
-  :ref:`Exporting Data <dli_01_0010>`
-  :ref:`Viewing Metadata <dli_01_0008>`
-  :ref:`Previewing Data <dli_01_0007>`

Difference Between DLI Tables and OBS Tables
--------------------------------------------

-  Data stored in DLI tables is applicable to delay-sensitive services, such as interactive queries.
-  Data stored in OBS tables is applicable to delay-insensitive services, such as historical data statistics and analysis.

Notes and Constraints
---------------------

-  **Database**

   -  **default** is the database built in DLI. You cannot create a database named **default**.
   -  DLI supports a maximum of 50 databases.

-  **Table**

   -  DLI supports a maximum of 5,000 tables.
   -  DLI supports the following table types:

      -  **MANAGED**: Data is stored in a DLI table.
      -  **EXTERNAL**: Data is stored in an OBS table.
      -  **View**: A view can only be created using SQL statements.
      -  Datasource table: The table type is also **EXTERNAL**.

   -  You cannot specify a storage path when creating a DLI table.

-  **Data import**

   -  Only OBS data can be imported to DLI or OBS.
   -  You can import data in CSV, Parquet, ORC, JSON, or Avro format from OBS to tables created on DLI.
   -  To import data in CSV format to a partitioned table, place the partition column in the last column of the data source.
   -  The encoding format of imported data can only be UTF-8.

-  **Data export**

   -  Data in DLI tables (whose table type is **MANAGED**) can only be exported to OBS buckets, and the export path must contain a folder.
   -  The exported file is in JSON format, and the text format can only be UTF-8.
   -  Data can be exported across accounts. That is, after account B authorizes account A, account A has the permission to read the metadata and permission information of account B's OBS bucket as well as the read and write permissions on the path. Account A can export data to the OBS path of account B.

Databases and Tables Page
-------------------------

The **Databases and Tables** page displays all created databases. You can view the database information, such as the owner and the number of tables.

.. table:: **Table 1** Database and table management parameters

   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                             |
   +===================================+=========================================================================================================================================================+
   | Database Name                     | -  The database name can contain only digits, letters, and underscores (_), but cannot contain only digits or start with an underscore (_).             |
   |                                   | -  The database name is case insensitive and cannot be left unspecified.                                                                                |
   |                                   | -  It cannot exceed 128 characters.                                                                                                                     |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Username                          | Database owner.                                                                                                                                         |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Tables                            | Number of tables in the database.                                                                                                                       |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Description                       | Description of the database specified during database creation. If no description is provided, **--** is displayed.                                     |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Enterprise Project                | Enterprise project to which the database belongs. An enterprise project facilitates project-level management and grouping of cloud resources and users. |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Operation                         | -  **Permissions**: View the permission information and perform user authorization, permission settings, and user permission revocation.                |
   |                                   | -  **Tables**: View the tables in the corresponding database. For details, see :ref:`Table Management Page <dli_01_0228__section4377195513315>`.        |
   |                                   | -  **Create Table**: This permission allows you to create a table in the corresponding database.                                                        |
   |                                   | -  **Modify Database**. This permission allows you to change the owner of the database. The username must exist under the same account.                 |
   |                                   | -  **Drop Database**: This permission allows you to delete the selected database.                                                                       |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_01_0228__section4377195513315:

Table Management Page
---------------------

From the **Data Management** page, click the database name or **Tables** in the **Operation** column to switch to the table management page.

The displayed page lists all tables created in the current database. You can view the table type, data storage location, and other information. Tables are listed in chronological order by default, with the most recently created tables displayed at the top.

.. table:: **Table 2** Table management parameters

   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                  |
   +===================================+==============================================================================================================================================================================+
   | Table Name                        | -  The table name can contain only digits, letters, and underscores (_), but cannot contain only digits or start with an underscore (_).                                     |
   |                                   | -  The table name is case insensitive and cannot be left unspecified.                                                                                                        |
   |                                   | -  The table name can contain the dollar sign ($). An example value is **$test**.                                                                                            |
   |                                   | -  It cannot exceed 128 characters.                                                                                                                                          |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Table Type                        | Table type. Available options are as follows:                                                                                                                                |
   |                                   |                                                                                                                                                                              |
   |                                   | -  **Managed**: Indicates that data is stored in a DLI table.                                                                                                                |
   |                                   | -  **External**: Indicates that data is stored in an OBS table.                                                                                                              |
   |                                   | -  **View**: Indicates the view type. You can only create views using SQL statements.                                                                                        |
   |                                   |                                                                                                                                                                              |
   |                                   |    .. note::                                                                                                                                                                 |
   |                                   |                                                                                                                                                                              |
   |                                   |       The table or view information contained in the view cannot be modified. If the table or view information is modified, the query may fail.                              |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Owner                             | User who creates the table.                                                                                                                                                  |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Storage Location                  | DLI, OBS, View, CloudTable, and CSS data location                                                                                                                            |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Size                              | Size of the data in the table. The value is displayed only for tables of the **Managed** type. For tables of other types, **--** is displayed.                               |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Data Source Path                  | -  If **Data Location** is **OBS**, the corresponding OBS path is displayed.                                                                                                 |
   |                                   | -  If **Data Location** is **DLI** and **View**, **--** is displayed.                                                                                                        |
   |                                   | -  When the data storage location is a datasource connection service such as CloudTable and CSS, the corresponding URL is displayed.                                         |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Created                           | Time when the table is created.                                                                                                                                              |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Last Accessed                     | Last time when an operation was performed on the table.                                                                                                                      |
   |                                   |                                                                                                                                                                              |
   |                                   | The last access time of a table refers only to the last time it was updated, not the time it was read (SELECT operation).                                                    |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Operation                         | -  **Manage Permissions**: This operation allows you to view the permission information and perform user authorization, permission settings, and user permission revocation. |
   |                                   | -  **More**:                                                                                                                                                                 |
   |                                   |                                                                                                                                                                              |
   |                                   |    -  **Delete**: Delete a table from the corresponding database.                                                                                                            |
   |                                   |    -  **Modify Owner**: Change the owner of a table The username must exist under the same account.                                                                          |
   |                                   |    -  **Import**: Import data stored in an OBS bucket to a DLI or OBS table.                                                                                                 |
   |                                   |    -  **Properties**: View data in **Metadata** and **Preview** tabs.                                                                                                        |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
