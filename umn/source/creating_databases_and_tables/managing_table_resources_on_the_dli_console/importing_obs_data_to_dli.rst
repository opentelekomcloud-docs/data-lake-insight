:original_name: dli_01_0253.html

.. _dli_01_0253:

Importing OBS Data to DLI
=========================

This section describes how to import data stored in OBS to a table on the DLI console.

Precautions
-----------

-  Only one path can be specified during data import. The path cannot contain commas (,).
-  To import data in CSV format to a partitioned table, place the column to be partitioned in the last column of the data source.
-  You are advised not to concurrently import data in to a table. If you concurrently import data into a table, there is a possibility that conflicts occur, leading to failed data import.
-  The imported file can be in CSV, Parquet, ORC, JSON, and Avro format. The encoding format must be UTF-8.

Prerequisites
-------------

The data to be imported has been stored on OBS.

Procedure
---------

#. You can import data on either the **Data Management** page or the **SQL Editor** page.

   -  To import data on the **Data Management** page:

      a. On the left of the management console, choose **Data Management** > **Databases and Tables**.
      b. Click the name of the database corresponding to the table where data is to be imported to switch to the table management page.
      c. Locate the row where the target table resides and choose **More** > **Import** in the **Operation** column. The **Import** dialog box is displayed.

   -  To import data on the **SQL Editor** page:

      a. On the left of the management console, click **SQL Editor**.
      b. In the navigation tree on the left of **SQL Editor**, click **Databases** to see all databases. Click the database where the target table belongs. The table list is displayed.
      c. Click |image1| on the right of the table and choose **Import** from the shortcut menu. The **Import** page is displayed.

#. In the **Import** dialog box, set the parameters based on :ref:`Table 1 <dli_01_0253__table48581581434>`.

   .. _dli_01_0253__table48581581434:

   .. table:: **Table 1** Description

      +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+
      | Parameter                        | Description                                                                                                                                                                                                                                      | Example                                   |
      +==================================+==================================================================================================================================================================================================================================================+===========================================+
      | Databases                        | Database where the current table is located.                                                                                                                                                                                                     | ``-``                                     |
      +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+
      | Table Name                       | Name of the current table.                                                                                                                                                                                                                       | ``-``                                     |
      +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+
      | Queues                           | Queue where the imported data will be used                                                                                                                                                                                                       | ``-``                                     |
      +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+
      | File Format                      | Format of the data source file to be imported. The CSV, Parquet, ORC, JSON, and Avro formats are supported. Encoding format. Only UTF-8 is supported.                                                                                            | CSV                                       |
      +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+
      | Path                             | You can directly enter a path or click |image2| and select an OBS path. If no bucket is available, you can directly switch to the OBS management console and create an OBS bucket.                                                               | obs://DLI/sampledata.csv                  |
      |                                  |                                                                                                                                                                                                                                                  |                                           |
      |                                  | -  **When creating an OBS table, you must specify a folder as the directory. If a file is specified, data import may be failed.**                                                                                                                |                                           |
      |                                  | -  **If a folder and a file have the same name in the OBS directory, the file path is preferred as the path of the data to be imported.**                                                                                                        |                                           |
      |                                  |                                                                                                                                                                                                                                                  |                                           |
      |                                  | .. note::                                                                                                                                                                                                                                        |                                           |
      |                                  |                                                                                                                                                                                                                                                  |                                           |
      |                                  |    The path can be a file or folder.                                                                                                                                                                                                             |                                           |
      +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+
      | Table Header: No/Yes             | This parameter is valid only when **File Format** is set to **CSV**. Whether the data source to be imported contains the table header.                                                                                                           | ``-``                                     |
      |                                  |                                                                                                                                                                                                                                                  |                                           |
      |                                  | Click **Advanced Settings** and select the checkbox next to **Table Header: No**. If the checkbox is selected, the table header is displayed. If the checkbox is deselected, no table header is displayed.                                       |                                           |
      +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+
      | User-defined Delimiter           | This parameter is valid only when **File Format** is set to **CSV** and you select **User-defined Delimiter**.                                                                                                                                   | Default value: comma (,)                  |
      |                                  |                                                                                                                                                                                                                                                  |                                           |
      |                                  | The following delimiters are supported:                                                                                                                                                                                                          |                                           |
      |                                  |                                                                                                                                                                                                                                                  |                                           |
      |                                  | -  Comma (,)                                                                                                                                                                                                                                     |                                           |
      |                                  | -  Vertical bar (|)                                                                                                                                                                                                                              |                                           |
      |                                  | -  Tab character (\\t)                                                                                                                                                                                                                           |                                           |
      |                                  | -  Others: Enter a user-defined delimiter.                                                                                                                                                                                                       |                                           |
      +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+
      | User-defined Quotation Character | This parameter is valid only when **File Format** is set to **CSV** and **User-defined Quotation Character** is selected.                                                                                                                        | Default value: double quotation marks (") |
      |                                  |                                                                                                                                                                                                                                                  |                                           |
      |                                  | The following quotation characters are supported:                                                                                                                                                                                                |                                           |
      |                                  |                                                                                                                                                                                                                                                  |                                           |
      |                                  | -  Single quotation mark (')                                                                                                                                                                                                                     |                                           |
      |                                  | -  Double quotation marks (")                                                                                                                                                                                                                    |                                           |
      |                                  | -  Others: Enter a user-defined quotation character.                                                                                                                                                                                             |                                           |
      +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+
      | User-defined Escape Character    | This parameter is valid only when **File Format** is set to **CSV** and you select **User-defined Escape Character**.                                                                                                                            | Default value: backslash (\\)             |
      |                                  |                                                                                                                                                                                                                                                  |                                           |
      |                                  | The following escape characters are supported:                                                                                                                                                                                                   |                                           |
      |                                  |                                                                                                                                                                                                                                                  |                                           |
      |                                  | -  Backslash (\\)                                                                                                                                                                                                                                |                                           |
      |                                  | -  Others: Enter a user-defined escape character.                                                                                                                                                                                                |                                           |
      +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+
      | Date Format                      | This parameter is valid only when **File Format** is set to **CSV** or **JSON**.                                                                                                                                                                 | 2000-01-01                                |
      |                                  |                                                                                                                                                                                                                                                  |                                           |
      |                                  | This parameter specifies the format of the date in the table and is valid only **Advanced Settings** is selected. The default value is **yyyy-MM-dd**. For definition of characters involved in the date pattern, see Table 3 in .               |                                           |
      +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+
      | Timestamp Format                 | This parameter is valid only when **File Format** is set to **CSV** or **JSON**.                                                                                                                                                                 | 2000-01-01 09:00:00                       |
      |                                  |                                                                                                                                                                                                                                                  |                                           |
      |                                  | This parameter specifies the format of the timestamp in the table and is valid only **Advanced Settings** is selected. The default value is **yyyy-MM-dd HH:mm:ss**. For definition of characters involved in the time pattern, see Table 3 in . |                                           |
      +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+
      | Error Records Path               | This parameter is valid only when **File Format** is set to **CSV** or **JSON**.                                                                                                                                                                 | obs://DLI/                                |
      |                                  |                                                                                                                                                                                                                                                  |                                           |
      |                                  | The parameter specifies the error data is stored in the corresponding OBS path and is valid only **Advanced Settings** is selected.                                                                                                              |                                           |
      +----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+

#. Click **OK**.

#. You can view the imported data in either of the following ways:

   .. note::

      Currently, only the first 10 records are displayed.

   -  Choose **Data Management** > **Databases and Tables** in the navigation pane of the console. Locate the row that contains the database where the target table belongs and click **More** > **View Properties** in the **Operation** column. In the displayed dialog box, click the **Preview** tab to view the imported data.
   -  On the **Databases** tab of the **SQL Editor**, click the database name to go to the table list. Click |image3| on the right of a table name and choose **View Properties** from the shortcut menu. In the displayed dialog box, click **Preview** to view the imported data.

#. (Optional) View the status and execution result of the importing job on the **Job Management** > **SQL Jobs** page.

.. |image1| image:: /_static/images/en-us_image_0237990324.png
.. |image2| image:: /_static/images/en-us_image_0206789903.png
.. |image3| image:: /_static/images/en-us_image_0237994911.png
