:original_name: dli_08_0100.html

.. _dli_08_0100:

Importing Data to the Table
===========================

Function
--------

The **LOAD DATA** function can be used to import data in **CSV**, **Parquet**, **ORC**, **JSON**, and **Avro** formats. The data is converted into the **Parquet** data format for storage.

Syntax
------

::

   LOAD DATA INPATH 'folder_path' INTO TABLE [db_name.]table_name
     OPTIONS(property_name=property_value, ...);

Keywords
--------

-  INPATH: path of data to be imported
-  OPTIONS: list of properties

Parameters
----------

.. table:: **Table 1** Parameters

   +-------------+--------------------------------------------------------------------------------------------+
   | Parameter   | Description                                                                                |
   +=============+============================================================================================+
   | folder_path | OBS path of the file or folder used for storing the raw data.                              |
   +-------------+--------------------------------------------------------------------------------------------+
   | db_name     | Enter the database name. If this parameter is not specified, the current database is used. |
   +-------------+--------------------------------------------------------------------------------------------+
   | table_name  | Name of the DLI table to which data is to be imported.                                     |
   +-------------+--------------------------------------------------------------------------------------------+

The following configuration options can be used during data import:

-  DATA_TYPE: specifies the type of data to be imported. Currently, **CSV**, **Parquet**, **ORC**, **JSON**, and **Avro** are supported. The default value is **CSV**.

   The configuration item is **OPTIONS** ('DATA_TYPE' = 'CSV').

   When importing a **CSV** file or a **JSON** file, you can select one of the following modes:

   -  **PERMISSIVE**: When the **PERMISSIVE** mode is selected, the data of a column is set to **null** if its data type does not match that of the target table column.
   -  **DROPMALFORMED**: When the **DROPMALFORMED** mode is selected, the data of a column s not imported if its data type does not match that of the target table column.
   -  **FAILFAST**: When the **FAILFAST** mode is selected, exceptions might occur and the import may fail if a column type does not match.

   You can set the mode by adding **OPTIONS ('MODE' = 'PERMISSIVE')** to the **OPTIONS** parameter.

-  **DELIMITER**: You can specify a separator in the import statement. The default value is **,**.

   The configuration item is **OPTIONS('DELIMITER'=',')**.

   For CSV data, the following delimiters are supported:

   -  Tab character, for example, **'DELIMITER'='\\t'**.
   -  Any binary character, for example, **'DELIMITER'='\\u0001(^A)'**.
   -  Single quotation mark ('). A single quotation mark must be enclosed in double quotation marks (" "). For example, **'DELIMITER'= "'"**.
   -  **\\001(^A)** and **\\017(^Q)** are also supported, for example, **'DELIMITER'='\\001(^A)'** and **'DELIMITER'='\\017(^Q)'**.

-  **QUOTECHAR**: You can specify quotation marks in the import statement. The default value is double quotation marks (**"**).

   The configuration item is **OPTIONS('QUOTECHAR'='"')**.

-  **COMMENTCHAR**: You can specify the comment character in the import statement. During the import operation, if a comment character is at the beginning of a row, the row is considered as a comment and will not be imported. The default value is a pound key (#).

   The configuration item is *OPTIONS('COMMENTCHAR'='#')*.

-  **HEADER**: Indicates whether the source file contains a header. Possible values can be **true** and **false**. **true** indicates that the source file contains a header, and **false** indicates that the source file does not contain a header. The default value is **false**. If no header exists, specify the **FILEHEADER** parameter in the **LOAD DATA** statement to add a header.

   The configuration item is *OPTIONS('HEADER'='true')*.

-  **FILEHEADER**: If the source file does not contain any header, add a header to the **LOAD DATA** statement.

   *OPTIONS('FILEHEADER'='column1,column2')*

-  **ESCAPECHAR**: Is used to perform strict verification of the escape character on CSV files. The default value is a slash (**\\\\**).

   The configuration item is OPTIONS. (ESCAPECHAR?=?\\\\?)

   .. note::

      Enter **ESCAPECHAR** in the CSV data. **ESCAPECHAR** must be enclosed in double quotation marks (" "). For example, "a\\b".

-  **MAXCOLUMNS**: This parameter is optional and specifies the maximum number of columns parsed by a CSV parser in a line.

   The configuration item is *OPTIONS('MAXCOLUMNS'='400')*.

   .. table:: **Table 2** MAXCOLUMNS

      ============================== ============= =============
      Name of the Optional Parameter Default Value Maximum Value
      ============================== ============= =============
      MAXCOLUMNS                     2000          20000
      ============================== ============= =============

   .. note::

      After the value of **MAXCOLUMNS Option** is set, data import will require the memory of **executor**. As a result, data may fail to be imported due to insufficient **executor** memory.

-  **DATEFORMAT**: Specifies the date format of a column.

   *OPTIONS('DATEFORMAT'='dateFormat')*

   .. note::

      -  The default value is yyyy-MM-dd.
      -  The date format is specified by the date mode string of **Java**. For the Java strings describing date and time pattern, characters **A** to **Z** and **a** to **z** without single quotation marks (') are interpreted as pattern characters , which are used to represent date or time string elements. If the pattern character is quoted by single quotation marks ('), text matching rather than parsing is performed. For the definition of pattern characters in Java, see :ref:`Table 3 <dli_08_0100__en-us_topic_0114776194_en-us_topic_0093946741_table489265920252>`.

   .. _dli_08_0100__en-us_topic_0114776194_en-us_topic_0093946741_table489265920252:

   .. table:: **Table 3** Definition of characters involved in the date and time patterns

      +-----------+-------------------------------+------------------------------------------+
      | Character | Date or Time Element          | Example                                  |
      +===========+===============================+==========================================+
      | G         | Epoch ID                      | AD                                       |
      +-----------+-------------------------------+------------------------------------------+
      | y         | Year                          | 1996; 96                                 |
      +-----------+-------------------------------+------------------------------------------+
      | M         | Month                         | July; Jul; 07                            |
      +-----------+-------------------------------+------------------------------------------+
      | w         | Number of the week in a year  | 27 (the twenty-seventh week of the year) |
      +-----------+-------------------------------+------------------------------------------+
      | W         | Number of the week in a month | 2 (the second week of the month)         |
      +-----------+-------------------------------+------------------------------------------+
      | D         | Number of the day in a year   | 189 (the 189th day of the year)          |
      +-----------+-------------------------------+------------------------------------------+
      | d         | Number of the day in a month  | 10 (the tenth day of the month)          |
      +-----------+-------------------------------+------------------------------------------+
      | u         | Number of the day in a week   | 1 (Monday), ..., 7 (Sunday)              |
      +-----------+-------------------------------+------------------------------------------+
      | a         | am/pm flag                    | pm (12:00-24:00)                         |
      +-----------+-------------------------------+------------------------------------------+
      | H         | Hour time (0-23)              | 2                                        |
      +-----------+-------------------------------+------------------------------------------+
      | h         | Hour time (1-12)              | 12                                       |
      +-----------+-------------------------------+------------------------------------------+
      | m         | Number of minutes             | 30                                       |
      +-----------+-------------------------------+------------------------------------------+
      | s         | Number of seconds             | 55                                       |
      +-----------+-------------------------------+------------------------------------------+
      | S         | Number of milliseconds        | 978                                      |
      +-----------+-------------------------------+------------------------------------------+
      | z         | Time zone                     | Pacific Standard Time; PST; GMT-08:00    |
      +-----------+-------------------------------+------------------------------------------+

-  **TIMESTAMPFORMAT**: Specifies the timestamp format of a column.

   *OPTIONS('TIMESTAMPFORMAT'='timestampFormat')*

   .. note::

      -  Default value: yyyy-MM-dd HH:mm:ss.
      -  The timestamp format is specified by the Java time pattern string. For details, see :ref:`Table 3 <dli_08_0100__en-us_topic_0114776194_en-us_topic_0093946741_table489265920252>`.

-  **Mode**: Specifies the processing mode of error records while importing. The options are as follows: **PERMISSIVE**, **DROPMALFORMED**, and **FAILFAST**.

   *OPTIONS('MODE'='permissive')*

   .. note::

      -  **PERMISSIVE (default)**: Parse bad records as much as possible. If a field cannot be converted, the entire row is null.
      -  **DROPMALFORMED**: Ignore the **bad records** that cannot be parsed.
      -  **FAILFAST**: If a record cannot be parsed, an exception is thrown and the job fails.

-  **BADRECORDSPATH**: Specifies the directory for storing error records during the import.

   *OPTIONS('BADRECORDSPATH'='obs://bucket/path')*

   .. note::

      It is recommended that this option be used together with the **DROPMALFORMED** pattern to import the records that can be successfully converted into the target table and store the records that fail to be converted to the specified error record storage directory.

Precautions
-----------

-  When importing or creating an OBS table, you must specify a folder as the directory. If a file is specified, data import may be failed.
-  Only the raw data stored in the OBS path can be imported.
-  You are advised not to concurrently import data in to a table. If you concurrently import data into a table, there is a possibility that conflicts occur, leading to failed data import.
-  Only one path can be specified during data import. The path cannot contain commas (,).
-  If a folder and a file with the same name exist in the OBS bucket directory, the data is preferentially to be imported directed to the file rather than the folder.
-  When importing data of the PARQUET, ORC, or JSON format, you must specify *DATA_TYPE*. Otherwise, the data is parsed into the default format **CSV**. In this case, the format of the imported data is incorrect.
-  If the data to be imported is in the CSV or JSON format and contains the date and columns, you need to specify *DATEFORMAT* and *TIMESTAMPFORMAT*. Otherwise, the data will be parsed into the default date and timestamp formats.

Example
-------

.. note::

   Before importing data, you must create a table. For details, see :ref:`Creating an OBS Table <dli_08_0223>` or :ref:`Creating a DLI Table <dli_08_0224>`.

-  To import a CSV file to a DLI table named **t**, run the following statement:

   ::

      LOAD DATA INPATH 'obs://dli/data.csv' INTO TABLE t
        OPTIONS('DELIMITER'=',' , 'QUOTECHAR'='"','COMMENTCHAR'='#','HEADER'='false');

-  To import a JSON file to a DLI table named **jsontb**, run the following statement:

   ::

      LOAD DATA INPATH 'obs://dli/alltype.json' into table jsontb
        OPTIONS('DATA_TYPE'='json','DATEFORMAT'='yyyy/MM/dd','TIMESTAMPFORMAT'='yyyy/MM/dd HH:mm:ss');
