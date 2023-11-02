:original_name: dli_08_0076.html

.. _dli_08_0076:

Creating an OBS Table Using the DataSource Syntax
=================================================

Function
--------

Create an OBS table using the DataSource syntax.

The main differences between the DataSource and the Hive syntax lie in the supported data formats and the number of supported partitions. For details, see syntax and precautions.

Usage
-----

-  The size of the table will not be calculated during table creation.
-  When data is added, the table size will be changed to 0.
-  You can view the table size on OBS.

Precautions
-----------

-  The table and column names are case-insensitive.

-  Descriptions of table names and column names support only string constants.

-  During table creation, you need to specify the column name and corresponding data type. The data type is primitive type.

-  If a folder and a file have the same name in the OBS directory, the file is preferred as the path when creating an OBS table.

-  During table creation, if the specified path is an OBS directory and it contains subdirectories (or nested subdirectories), all file types and content in the subdirectories are considered table content.

   Ensure that all file types in the specified directory and its subdirectories are consistent with the storage format specified in the table creation statement. All file content must be consistent with the fields in the table. Otherwise, errors will be reported in the query.

   You can set **multiLevelDirEnable** to **true** in the **OPTIONS** statement to query the content in the subdirectory. The default value is **false** (Note that this configuration item is a table attribute, exercise caution when performing this operation). Hive tables do not support this configuration item.

-  The OBS storage path must be a directory on the OBS. The directory must be created in advance and be empty.

-  When a partitioned table is created, the column specified in PARTITIONED BY must be a column in the table, and the partition type must be specified. The partition column supports only the **string**, **boolean**, **tinyint**, **smallint**, **short**, **int**, **bigint**, **long**, **decimal**, **float**, **double**, **date**, and **timestamp** type.

-  When a partitioned table is created, the partition field must be the last one or several fields of the table field, and the sequence of the partition fields must be the same. Otherwise, an error occurs.

-  A maximum of 7,000 partitions can be created in a single table.

-  The CREATE TABLE AS statement cannot specify table attributes or create partitioned tables.

Syntax
------

::

   CREATE TABLE [IF NOT EXISTS] [db_name.]table_name
     [(col_name1 col_type1 [COMMENT col_comment1], ...)]
     USING file_format
     [OPTIONS (path 'obs_path', key1=val1, key2=val2, ...)]
     [PARTITIONED BY (col_name1, col_name2, ...)]
     [COMMENT table_comment]
     [AS select_statement];

Keyword
-------

-  IF NOT EXISTS: Prevents system errors when the created table exists.
-  USING: Specifies the storage format.
-  OPTIONS: Specifies the attribute name and attribute value when a table is created.
-  COMMENT: Field or table description.
-  PARTITIONED BY: Partition field.
-  AS: Run the CREATE TABLE AS statement to create a table.

Parameter
---------

.. table:: **Table 1** Parameter description

   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                                                                                             |
   +===================================+=========================================================================================================================================================================================================================================================+
   | db_name                           | Database name                                                                                                                                                                                                                                           |
   |                                   |                                                                                                                                                                                                                                                         |
   |                                   | The value can contain letters, numbers, and underscores (_), but cannot contain only numbers or start with a number or underscore (_).                                                                                                                  |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name                        | Name of the table to be created in the database                                                                                                                                                                                                         |
   |                                   |                                                                                                                                                                                                                                                         |
   |                                   | The value can contain letters, numbers, and underscores (_), but cannot contain only numbers or start with a number or underscore (_). The matching rule is **^(?!_)(?![0-9]+$)[A-Za-z0-9_$]*$**.                                                       |
   |                                   |                                                                                                                                                                                                                                                         |
   |                                   | Special characters must be enclosed in single quotation marks ('').                                                                                                                                                                                     |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | col_name                          | Column names with data types separated by commas (,)                                                                                                                                                                                                    |
   |                                   |                                                                                                                                                                                                                                                         |
   |                                   | The column name contains letters, digits, and underscores (_). It cannot contain only digits and must contain at least one letter.                                                                                                                      |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | col_type                          | Data type of a column field                                                                                                                                                                                                                             |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | col_comment                       | Column field description                                                                                                                                                                                                                                |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | file_format                       | Input format of the table. The value can be **orc**, **parquet**, **json**, **csv**, or **avro**.                                                                                                                                                       |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | path                              | OBS storage path where data files are stored                                                                                                                                                                                                            |
   |                                   |                                                                                                                                                                                                                                                         |
   |                                   | Format: **obs://bucketName/tblPath**                                                                                                                                                                                                                    |
   |                                   |                                                                                                                                                                                                                                                         |
   |                                   | *bucketName*: bucket name                                                                                                                                                                                                                               |
   |                                   |                                                                                                                                                                                                                                                         |
   |                                   | *tblPath*: directory name. You do not need to specify the file name following the directory.                                                                                                                                                            |
   |                                   |                                                                                                                                                                                                                                                         |
   |                                   | For details about attribute names and values during table creation, see :ref:`Table 2 <dli_08_0076__en-us_topic_0114776170_table1376011233214>`.                                                                                                        |
   |                                   |                                                                                                                                                                                                                                                         |
   |                                   | For details about the table attribute names and values when **file_format** is set to **csv**, see :ref:`Table 2 <dli_08_0076__en-us_topic_0114776170_table1376011233214>` and :ref:`Table 3 <dli_08_0076__en-us_topic_0114776170_table1876517231928>`. |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_comment                     | Description of the table                                                                                                                                                                                                                                |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | select_statement                  | The **CREATE TABLE AS** statement is used to insert the **SELECT** query result of the source table or a data record to a new table in OBS bucket.                                                                                                      |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_08_0076__en-us_topic_0114776170_table1376011233214:

.. table:: **Table 2** OPTIONS parameter description

   +---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
   | Parameter           | Description                                                                                                                                                                                                                               | Default Value |
   +=====================+===========================================================================================================================================================================================================================================+===============+
   | path                | Specified table storage location. Currently, only OBS is supported.                                                                                                                                                                       | ``-``         |
   +---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
   | multiLevelDirEnable | Whether to iteratively query data in subdirectories when subdirectories are nested. When this parameter is set to **true**, all files in the table path, including files in subdirectories, are iteratively read when a table is queried. | false         |
   +---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
   | dataDelegated       | Whether to clear data in the path when deleting a table or partition                                                                                                                                                                      | false         |
   +---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
   | compression         | Specified compression format. Generally, you need to set this parameter to **zstd** for parquet files.                                                                                                                                    | ``-``         |
   +---------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+

When the file format is set to **CSV**, you can set the following OPTIONS parameters:

.. _dli_08_0076__en-us_topic_0114776170_table1876517231928:

.. table:: **Table 3** OPTIONS parameter description of the CSV data format

   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------+
   | Parameter             | Description                                                                                                                                                                                   | Default Value                |
   +=======================+===============================================================================================================================================================================================+==============================+
   | delimiter             | Data separator                                                                                                                                                                                | Comma (,)                    |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------+
   | quote                 | Quotation character                                                                                                                                                                           | Double quotation marks (" ") |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------+
   | escape                | Escape character                                                                                                                                                                              | Backslash (\\)               |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------+
   | multiLine             | Whether the column data contains carriage return characters or transfer characters. The value **true** indicates yes and the value **false** indicates no.                                    | false                        |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------+
   | dateFormat            | Date format of the **date** field in a CSV file                                                                                                                                               | yyyy-MM-dd                   |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------+
   | timestampFormat       | Date format of the **timestamp** field in a CSV file                                                                                                                                          | yyyy-MM-dd HH:mm:ss          |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------+
   | mode                  | Mode for parsing CSV files. The options are as follows:                                                                                                                                       | PERMISSIVE                   |
   |                       |                                                                                                                                                                                               |                              |
   |                       | -  **PERMISSIVE**: Permissive mode. If an incorrect field is encountered, set the line to **Null**.                                                                                           |                              |
   |                       | -  **DROPMALFORMED**: When an incorrect field is encountered, the entire line is discarded.                                                                                                   |                              |
   |                       | -  **FAILFAST**: Error mode. If an error occurs, it is automatically reported.                                                                                                                |                              |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------+
   | header                | Whether CSV contains header information. The value **true** indicates that the table header information is contained, and the value **false** indicates that the information is not included. | false                        |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------+
   | nullValue             | Character that represents the null value. For example, **nullValue= "\\\\N"** indicates that **\\N** represents the null value.                                                               | ``-``                        |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------+
   | comment               | Character that indicates the beginning of the comment. For example, **comment= '#'** indicates that the line starting with **#** is a comment.                                                | ``-``                        |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------+
   | compression           | Data compression format. Currently, **gzip**, **bzip2**, and **deflate** are supported. If you do not want to compress data, enter **none**.                                                  | none                         |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------+
   | encoding              | Data encoding format. Available values are **utf-8**, **gb2312**, and **gbk**. Value **utf-8** will be used if this parameter is left empty.                                                  | utf-8                        |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------+

Example
-------

-  Create a **parquetTable** OBS table.

   ::

      CREATE TABLE parquetTable (name string, id int) USING parquet OPTIONS (path "obs://bucketName/filePath");

-  Create a **parquetZstdTable** OBS table and set the compression format to **zstd**.

   .. code-block::

      CREATE TABLE parquetZstdTable (name string, id string) USING parquet OPTIONS (path "obs://bucketName/filePath",compression='zstd');

-  Create a **student** table that has two fields **name** and **score**\ and partition the table by **classNo**.

   ::

      CREATE TABLE IF NOT EXISTS student(name STRING, score DOUBLE, classNo INT) USING csv OPTIONS (PATH 'obs://bucketName/filePath') PARTITIONED BY (classNo);

   .. note::

      The **classNo** field is a partition field and must be placed at the end of the table field, that is, **student(name STRING, score DOUBLE, classNo INT)**.

-  To create table **t1** and insert data of table **t2** into table **t1**, run the following statement:

   ::

      CREATE TABLE t1 USING parquet OPTIONS(path 'obs://bucketName/tblPath') AS select * from t2;
