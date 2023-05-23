:original_name: dli_01_0005.html

.. _dli_01_0005:

Creating a Database or a Table
==============================

Definition of Database and Table in DLI
---------------------------------------

A database, built on the computer storage device, is a data warehouse where data is organized, stored, and managed based on its structure.

The table is an important part of the database. It consists of rows and columns. Each column functions as a field. Each value in a field (column) represents a type of data.

The database is a framework and the table contains data content. A database has one or more tables.

You can create databases and tables on the management console or using SQL statements. This section describes how to create a database and a table on the management console.

.. note::

   A view can be created only by using SQL statements. You cannot create a view on the **Create Table** page.

Precautions
-----------

-  If a folder and a file have the same name in the OBS directory, the file path is preferred as the path of the OBS table to be created.

Creating a Database
-------------------

#. You can create a database on either the **Data Management** page or the **SQL Editor** page.

   -  To create a database on the **Data Management** page:

      a. On the left of the management console, choose **Data Management** > **Databases and Tables**.
      b. In the upper right corner of the **Databases and Tables** page, click **Create Database** to create a database.

   -  To create a database on the **SQL Editor** page:

      a. On the left of the management console, click **SQL Editor**.
      b. In the navigation pane on the left, click |image1| beside **Databases**.

#. In the displayed **Create Database** dialog box, specify **Name** and **Description** by referring to :ref:`Table 1 <dli_01_0005__table055917491187>`.

   .. _dli_01_0005__table055917491187:

   .. table:: **Table 1** Description

      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter             | Description                                                                                                                                 | Example               |
      +=======================+=============================================================================================================================================+=======================+
      | Database Name         | -  The database name can contain only digits, letters, and underscores (_), but cannot contain only digits or start with an underscore (_). | DB01                  |
      |                       | -  The database name is case insensitive and cannot be left blank.                                                                          |                       |
      |                       | -  The length of the database name cannot exceed 128 characters.                                                                            |                       |
      |                       |                                                                                                                                             |                       |
      |                       | .. note::                                                                                                                                   |                       |
      |                       |                                                                                                                                             |                       |
      |                       |    The **default** database is a built-in database. You cannot create the **default**. database.                                            |                       |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Description           | Description of a database.                                                                                                                  | ``-``                 |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

#. Click **OK**.

   After a database is created, you can view and select the database for use on the **Databases and Tables** page or **SQL Editor** page.

Creating a Table
----------------

Before creating a table, ensure that a database has been created.

#. You can create a table on either the **Databases and Tables** page or the **SQL Editor** page.

   .. note::

      Datasource connection tables, such as View tables, HBase (MRS) tables, OpenTSDB (MRS) tables, DWS tables, RDS tables, and CSS tables, cannot be created. You can use SQL to create views and datasource connection tables. For details, see sections **Creating a View** and **Creating a Datasource Connection Table** in the *Data Lake Insight SQL Syntax Reference*.

   -  To create a table on the **Data Management** page:

      a. On the left of the management console, choose **Data Management** > **Databases and Tables**.
      b. On the **Databases and Tables** page, select the database for which you want to create a table. In the **Operation** column, click **More** > **Create Table** to create a table in the current database.

   -  To create a table on the **SQL Editor** page:

      a. On the left of the management console, click **SQL Editor**.
      b. In the navigation pane of the displayed **SQL Editor** page, click **Databases**. You can create a table in either of the following ways:

         -  Click a database name. In the **Tables** area, click |image2| on the right to create a table in the current database.
         -  Click |image3| on the right of the database and choose **Create Table** from the shortcut menu to create a table in the current database.

#. In the displayed **Create Table** dialog box, set parameters as required.

   -  If you set **Data Location** to **DLI**, set related parameters by referring to :ref:`Table 2 <dli_01_0005__table34159998103738>`.

   -  If you set **Data Location** to **OBS**, set related parameters by referring to :ref:`Table 2 <dli_01_0005__table34159998103738>` and :ref:`Table 3 <dli_01_0005__table1913602718314>`.

      .. _dli_01_0005__table34159998103738:

      .. table:: **Table 2** Common parameters

         +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Parameter             | Description                                                                                                                                                                            | Example               |
         +=======================+========================================================================================================================================================================================+=======================+
         | Table Name            | -  The table name can contain only digits, letters, and underscores (_), but cannot contain only digits or start with an underscore (_).                                               | table01               |
         |                       | -  The table name is case insensitive and cannot be left unspecified.                                                                                                                  |                       |
         |                       | -  The table name can contain the dollar sign ($). An example value is **$test**.                                                                                                      |                       |
         |                       | -  The length of the table name cannot exceed 128 characters.                                                                                                                          |                       |
         +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Data Location         | Data storage location. Currently, DLI and OBS are supported.                                                                                                                           | DLI                   |
         +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Description           | Description of the table.                                                                                                                                                              | ``-``                 |
         +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Column Type           | Available values: **Normal** or **Partition**                                                                                                                                          | Normal                |
         +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Column                | Name of a column in a table. The column name must contain at least one letter and can contain underscores (_). It cannot contain only digits.                                          | name                  |
         |                       |                                                                                                                                                                                        |                       |
         |                       | You can select **Normal** or **Partition**. Partition columns are dedicated to partition tables. User data is partitioned to improve query efficiency.                                 |                       |
         |                       |                                                                                                                                                                                        |                       |
         |                       | .. note::                                                                                                                                                                              |                       |
         |                       |                                                                                                                                                                                        |                       |
         |                       |    The column name is case-insensitive and must be unique.                                                                                                                             |                       |
         +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Type                  | Data type of a column. This parameter corresponds to **Column Name**.                                                                                                                  | string                |
         |                       |                                                                                                                                                                                        |                       |
         |                       | -  **string**: The data is of the string type.                                                                                                                                         |                       |
         |                       | -  **int**: Each integer is stored on four bytes.                                                                                                                                      |                       |
         |                       | -  **date**: The value ranges from 0000-01-01 to 9999-12-31.                                                                                                                           |                       |
         |                       | -  **double**: Each number is stored on eight bytes.                                                                                                                                   |                       |
         |                       | -  **boolean**: Each value is stored on one byte.                                                                                                                                      |                       |
         |                       | -  **decimal**: The valid bits are positive integers between 1 to 38, including 1 and 38. The decimal digits are integers less than 10.                                                |                       |
         |                       | -  **smallint/short**: The number is stored on two bytes.                                                                                                                              |                       |
         |                       | -  **bigint/long**: The number is stored on eight bytes.                                                                                                                               |                       |
         |                       | -  **timestamp**: The data indicates a date and time. The value can be accurate to six decimal points.                                                                                 |                       |
         |                       | -  **float**: Each number is stored on four bytes.                                                                                                                                     |                       |
         |                       | -  **tinyint**: Each number is stored on one byte. Only OBS tables support this data type.                                                                                             |                       |
         +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Column Description    | Description of a column.                                                                                                                                                               | ``-``                 |
         +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Operation             | -  Add Column                                                                                                                                                                          | ``-``                 |
         |                       | -  Delete                                                                                                                                                                              |                       |
         |                       |                                                                                                                                                                                        |                       |
         |                       |    .. note::                                                                                                                                                                           |                       |
         |                       |                                                                                                                                                                                        |                       |
         |                       |       If the table to be created includes a great number of columns, you are advised to use SQL statements to create the table or import column information from the local EXCEL file. |                       |
         +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

      .. _dli_01_0005__table1913602718314:

      .. table:: **Table 3** Parameter description when **Data Location** is set to **OBS**

         +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
         | Parameter                        | Description                                                                                                                                                                                                                                      | Example                   |
         +==================================+==================================================================================================================================================================================================================================================+===========================+
         | Data Format                      | DLI supports the following data formats:                                                                                                                                                                                                         | CSV                       |
         |                                  |                                                                                                                                                                                                                                                  |                           |
         |                                  | -  **Parquet**: DLI can read non-compressed data or data that is compressed using Snappy and gzip.                                                                                                                                               |                           |
         |                                  | -  **CSV**: DLI can read non-compressed data or data that is compressed using gzip.                                                                                                                                                              |                           |
         |                                  | -  **ORC**: DLI can read non-compressed data or data that is compressed using Snappy.                                                                                                                                                            |                           |
         |                                  | -  **JSON**: DLI can read non-compressed data or data that is compressed using gzip.                                                                                                                                                             |                           |
         |                                  | -  **Avro**: DLI can read uncompressed Avro data.                                                                                                                                                                                                |                           |
         +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
         | Storage Path                     | Enter or select an OBS path. The path can be a folder or a path.                                                                                                                                                                                 | obs://obs1/sampledata.csv |
         |                                  |                                                                                                                                                                                                                                                  |                           |
         |                                  | .. note::                                                                                                                                                                                                                                        |                           |
         |                                  |                                                                                                                                                                                                                                                  |                           |
         |                                  |    If you need to import data stored in OBS to the OBS table, set this parameter to the path of a folder. If the table creation path is a file, data fails to be imported.                                                                       |                           |
         +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
         | Table Header: No/Yes             | This parameter is valid only when **Data Format** is set to **CSV**. Whether the data source to be imported contains the table header.                                                                                                           | ``-``                     |
         |                                  |                                                                                                                                                                                                                                                  |                           |
         |                                  | Click **Advanced Settings** and select the check box next to **Table Header: No**. If the check box is selected, the table header is displayed. If the check box is deselected, no table header is displayed.                                    |                           |
         +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
         | User-defined Delimiter           | This parameter is valid only when **Data Format** is set to **CSV** and you select **User-defined Delimiter**.                                                                                                                                   | Comma (,)                 |
         |                                  |                                                                                                                                                                                                                                                  |                           |
         |                                  | The following delimiters are supported:                                                                                                                                                                                                          |                           |
         |                                  |                                                                                                                                                                                                                                                  |                           |
         |                                  | -  Comma (,)                                                                                                                                                                                                                                     |                           |
         |                                  | -  Vertical bar (|)                                                                                                                                                                                                                              |                           |
         |                                  | -  Tab character (\\t)                                                                                                                                                                                                                           |                           |
         |                                  | -  Others: Enter a user-defined delimiter.                                                                                                                                                                                                       |                           |
         +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
         | User-defined Quotation Character | This parameter is valid only when **Data Format** is set to **CSV** and you select **User-defined Quotation Character**.                                                                                                                         | Single quotation mark (') |
         |                                  |                                                                                                                                                                                                                                                  |                           |
         |                                  | The following quotation characters are supported:                                                                                                                                                                                                |                           |
         |                                  |                                                                                                                                                                                                                                                  |                           |
         |                                  | -  Single quotation mark (')                                                                                                                                                                                                                     |                           |
         |                                  | -  Double quotation marks (")                                                                                                                                                                                                                    |                           |
         |                                  | -  Others: Enter a user-defined quotation character.                                                                                                                                                                                             |                           |
         +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
         | User-defined Escape Character    | This parameter is valid only when **Data Format** is set to **CSV** and you select **User-defined Escape Character**.                                                                                                                            | Backslash (\\)            |
         |                                  |                                                                                                                                                                                                                                                  |                           |
         |                                  | The following escape characters are supported:                                                                                                                                                                                                   |                           |
         |                                  |                                                                                                                                                                                                                                                  |                           |
         |                                  | -  Backslash (\\)                                                                                                                                                                                                                                |                           |
         |                                  | -  Others: Enter a user-defined escape character.                                                                                                                                                                                                |                           |
         +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
         | Date Format                      | This parameter is valid only when **Data Format** is set to **CSV** or **JSON**.                                                                                                                                                                 | 2000-01-01                |
         |                                  |                                                                                                                                                                                                                                                  |                           |
         |                                  | This parameter specifies the format of the date in the table and is valid only **Advanced Settings** is selected. The default value is **yyyy-MM-dd**. For definition of characters involved in the date pattern, see Table 3 in .               |                           |
         +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+
         | Timestamp Format                 | This parameter is valid only when **Data Format** is set to **CSV** or **JSON**.                                                                                                                                                                 | 2000-01-01 09:00:00       |
         |                                  |                                                                                                                                                                                                                                                  |                           |
         |                                  | This parameter specifies the format of the timestamp in the table and is valid only **Advanced Settings** is selected. The default value is **yyyy-MM-dd HH:mm:ss**. For definition of characters involved in the time pattern, see Table 3 in . |                           |
         +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------+

#. Click **OK**.

   After a table is created, you can view and select the table for use on the **Data Management** page or **SQL Editor** page.

#. (Optional) After a DLI table is created, you can decide whether to directly import data to the table.

.. |image1| image:: /_static/images/en-us_image_0237539077.png
.. |image2| image:: /_static/images/en-us_image_0237539075.png
.. |image3| image:: /_static/images/en-us_image_0237532018.png
