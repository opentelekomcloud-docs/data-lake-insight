:original_name: dli_08_0076.html

.. _dli_08_0076:

Creating an OBS Table Using the DataSource Syntax
=================================================

Function
--------

Create an OBS table using the DataSource syntax.

The main differences between the DataSource and the Hive syntax lie in the supported data formats and the number of supported partitions. For details, see syntax and precautions.

.. note::

   You are advised to use the OBS parallel file system for storage. A parallel file system is a high-performance file system that provides latency in milliseconds, TB/s-level bandwidth, and millions of IOPS. It applies to interactive big data analysis scenarios.

Precautions
-----------

-  The size of a table is not calculated when the table is created.

-  When data is added, the table size will be changed to 0.

-  You can check the table size on OBS.

-  Table properties cannot be specified using CTAS table creation statements.

-  **An OBS directory containing subdirectories:**

   If you specify an OBS directory that contains subdirectories when creating a table, all file types and content within those subdirectories will also be included as table content.

   Ensure that all file types in the specified directory and its subdirectories are consistent with the storage format specified in the table creation statement. All file content must be consistent with the fields in the table. Otherwise, errors will be reported in the query.

   You can set **multiLevelDirEnable** to **true** in the **OPTIONS** statement to query the content in the subdirectory. The default value is **false** (Note that this configuration item is a table attribute, exercise caution when performing this operation. Hive tables do not support this configuration item.)

-  **Instructions on using partitioned tables:**

   -  When a partitioned table is created, the column specified in PARTITIONED BY must be a column in the table, and the partition type must be specified. The partition column supports only the **string**, **boolean**, **tinyint**, **smallint**, **short**, **int**, **bigint**, **long**, **decimal**, **float**, **double**, **date**, and **timestamp** type.
   -  When a partitioned table is created, the partition field must be the last one or several fields of the table field, and the sequence of the partition fields must be the same. Otherwise, an error occurs.
   -  A maximum of 200,000 partitions can be created in a single table.
   -  CTAS table creation statements cannot be used to create partitioned tables.

Syntax
------

::

   CREATE TABLE [IF NOT EXISTS] [db_name.]table_name
     [(col_name1 col_type1 [COMMENT col_comment1], ...)]
     USING file_format
     [OPTIONS (path 'obs_path', key1=val1, key2=val2, ...)]
     [PARTITIONED BY (col_name1, col_name2, ...)]
     [COMMENT table_comment]
     [AS select_statement]

Keywords
--------

-  **IF NOT EXISTS**: Prevents system errors when the created table exists.
-  **USING**: Storage format.
-  **OPTIONS**: Property name and property value when a table is created.
-  **COMMENT**: Field or table description.
-  **PARTITIONED BY**: Partition field.
-  **AS**: Run the **CREATE TABLE AS** statement to create a table.

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                                                                                                                                        |
   +=======================+=======================+====================================================================================================================================================================================================================================================================================+
   | db_name               | No                    | Database name                                                                                                                                                                                                                                                                      |
   |                       |                       |                                                                                                                                                                                                                                                                                    |
   |                       |                       | The value can contain letters, numbers, and underscores (_), but it cannot contain only numbers or start with a number or underscore (_).                                                                                                                                          |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name            | Yes                   | Name of the table to be created in the database                                                                                                                                                                                                                                    |
   |                       |                       |                                                                                                                                                                                                                                                                                    |
   |                       |                       | The value can contain letters, numbers, and underscores (_), but it cannot contain only numbers or start with a number or underscore (_). The matching rule is **^(?!_)(?![0-9]+$)[A-Za-z0-9_$]*$**.                                                                               |
   |                       |                       |                                                                                                                                                                                                                                                                                    |
   |                       |                       | Special characters must be enclosed in single quotation marks ('').                                                                                                                                                                                                                |
   |                       |                       |                                                                                                                                                                                                                                                                                    |
   |                       |                       | The table name is case insensitive.                                                                                                                                                                                                                                                |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | col_name              | Yes                   | Column names with data types separated by commas (,)                                                                                                                                                                                                                               |
   |                       |                       |                                                                                                                                                                                                                                                                                    |
   |                       |                       | The column name can contain letters, numbers, and underscores (_), but it cannot contain only numbers and must contain at least one letter.                                                                                                                                        |
   |                       |                       |                                                                                                                                                                                                                                                                                    |
   |                       |                       | The column name is case insensitive.                                                                                                                                                                                                                                               |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | col_type              | Yes                   | Data type of a column field, which is primitive.                                                                                                                                                                                                                                   |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | col_comment           | No                    | Column field description, which can only be string constants.                                                                                                                                                                                                                      |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | file_format           | Yes                   | Format of the table to be created, which can be **orc**, **parquet**, **json**, **csv**, or **avro**.                                                                                                                                                                              |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | path                  | Yes                   | OBS storage path where data files are stored. You are advised to use an OBS parallel file system for storage.                                                                                                                                                                      |
   |                       |                       |                                                                                                                                                                                                                                                                                    |
   |                       |                       | Format: **obs://bucketName/tblPath**                                                                                                                                                                                                                                               |
   |                       |                       |                                                                                                                                                                                                                                                                                    |
   |                       |                       | *bucketName*: bucket name                                                                                                                                                                                                                                                          |
   |                       |                       |                                                                                                                                                                                                                                                                                    |
   |                       |                       | *tblPath*: directory name. You do not need to specify the file name following the directory.                                                                                                                                                                                       |
   |                       |                       |                                                                                                                                                                                                                                                                                    |
   |                       |                       | Refer to :ref:`Table 2 <dli_08_0076__dli_08_0076_en-us_topic_0114776170_table1376011233214>` for details about property names and values during table creation.                                                                                                                    |
   |                       |                       |                                                                                                                                                                                                                                                                                    |
   |                       |                       | Refer to :ref:`Table 2 <dli_08_0076__dli_08_0076_en-us_topic_0114776170_table1376011233214>` and :ref:`Table 3 <dli_08_0076__dli_08_0076_en-us_topic_0114776170_table1876517231928>` for details about the table property names and values when **file_format** is set to **csv**. |
   |                       |                       |                                                                                                                                                                                                                                                                                    |
   |                       |                       | If there is a folder and a file with the same name in the OBS directory, the path pointed to by the OBS table will prioritize the file over the folder.                                                                                                                            |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_comment         | No                    | Table description, which can only be string constants.                                                                                                                                                                                                                             |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | select_statement      | No                    | Used in the **CREATE TABLE AS** statement to insert the **SELECT** query results of the source table or a data record to a table newly created in the OBS bucket.                                                                                                                  |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_08_0076__dli_08_0076_en-us_topic_0114776170_table1376011233214:

.. table:: **Table 2** OPTIONS parameters

   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                                                                                                       |
   +=======================+=======================+===================================================================================================================================================================================================================================================+
   | path                  | No                    | Path where the table is stored, which currently can only be an OBS directory                                                                                                                                                                      |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | multiLevelDirEnable   | No                    | Whether data in subdirectories is iteratively queried when there are nested subdirectories. When this parameter is set to **true**, all files in the table path, including files in subdirectories, are iteratively read when a table is queried. |
   |                       |                       |                                                                                                                                                                                                                                                   |
   |                       |                       | Default value: **false**                                                                                                                                                                                                                          |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dataDelegated         | No                    | Whether data in the path is cleared when deleting a table or partition                                                                                                                                                                            |
   |                       |                       |                                                                                                                                                                                                                                                   |
   |                       |                       | Default value: **false**                                                                                                                                                                                                                          |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | compression           | No                    | Compression format. This parameter is typically required for Parquet files and is set to **zstd**.                                                                                                                                                |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

When **file_format** is set to **csv**, you can set the following OPTIONS parameters:

.. _dli_08_0076__dli_08_0076_en-us_topic_0114776170_table1876517231928:

.. table:: **Table 3** OPTIONS parameters of the CSV data format

   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                                                            |
   +=======================+=======================+========================================================================================================================================================================================================+
   | delimiter             | No                    | Data separator                                                                                                                                                                                         |
   |                       |                       |                                                                                                                                                                                                        |
   |                       |                       | Default value: comma (,)                                                                                                                                                                               |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | quote                 | No                    | Quotation character                                                                                                                                                                                    |
   |                       |                       |                                                                                                                                                                                                        |
   |                       |                       | Default value: double quotation marks ("")                                                                                                                                                             |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | escape                | No                    | Escape character                                                                                                                                                                                       |
   |                       |                       |                                                                                                                                                                                                        |
   |                       |                       | Default value: backslash (\\)                                                                                                                                                                          |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | multiLine             | No                    | Whether the column data contains carriage return characters or transfer characters. The value **true** indicates yes and the value **false** indicates no.                                             |
   |                       |                       |                                                                                                                                                                                                        |
   |                       |                       | Default value: **false**                                                                                                                                                                               |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dateFormat            | No                    | Date format of the **date** field in a CSV file                                                                                                                                                        |
   |                       |                       |                                                                                                                                                                                                        |
   |                       |                       | Default value: yyyy-MM-dd                                                                                                                                                                              |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | timestampFormat       | No                    | Date format of the **timestamp** field in a CSV file                                                                                                                                                   |
   |                       |                       |                                                                                                                                                                                                        |
   |                       |                       | Default value:                                                                                                                                                                                         |
   |                       |                       |                                                                                                                                                                                                        |
   |                       |                       | yyyy-MM-dd HH:mm:ss                                                                                                                                                                                    |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | mode                  | No                    | Mode for parsing CSV files. The options are as follows: Default value: **PERMISSIVE**                                                                                                                  |
   |                       |                       |                                                                                                                                                                                                        |
   |                       |                       | -  **PERMISSIVE**: Permissive mode. If an incorrect field is encountered, set the line to **Null**.                                                                                                    |
   |                       |                       | -  **DROPMALFORMED**: When an incorrect field is encountered, the entire line is discarded.                                                                                                            |
   |                       |                       | -  **FAILFAST**: Error mode. If an error occurs, it is automatically reported.                                                                                                                         |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | header                | No                    | Whether the CSV file contains header information. The value **true** indicates that the table header information is contained, and the value **false** indicates that the information is not included. |
   |                       |                       |                                                                                                                                                                                                        |
   |                       |                       | Default value: **false**                                                                                                                                                                               |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | nullValue             | No                    | Character that represents the null value. For example, **nullValue="nl"** indicates that **nl** represents the null value.                                                                             |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | comment               | No                    | Character that indicates the beginning of the comment. For example, **comment= '#'** indicates that the line starting with **#** is a comment.                                                         |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | compression           | No                    | Data compression format. Currently, **gzip**, **bzip2**, and **deflate** are supported. If you do not want to compress data, enter **none**.                                                           |
   |                       |                       |                                                                                                                                                                                                        |
   |                       |                       | Default value: **none**                                                                                                                                                                                |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encoding              | No                    | Data encoding format. Available values are **utf-8**, **gb2312**, and **gbk**. Value **utf-8** will be used if this parameter is left empty.                                                           |
   |                       |                       |                                                                                                                                                                                                        |
   |                       |                       | Default value: **utf-8**                                                                                                                                                                               |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_08_0076__section175482343414:

Example 1: Creating an OBS Non-Partitioned Table
------------------------------------------------

Example description: Create an OBS non-partitioned table named **table1** and use the **USING** keyword to set the storage format of the table to **orc**.

You can store OBS tables in **parquet**, **json**, or **avro** format.

::

   CREATE TABLE IF NOT EXISTS table1 (
       col_1   STRING,
       col_2   INT)
   USING orc
   OPTIONS (path 'obs://bucketName/filePath');

Example 2: Creating an OBS Partitioned Table
--------------------------------------------

Example description: Create a partitioned table named **student**. The partitioned table is partitioned using **facultyNo** and **classNo**. The **student** table is partitioned by faculty number (**facultyNo**) and class number (**classNo**).

In practice, you can select a proper partitioning field and add it to the brackets following the **PARTITIONED BY** keyword.

::

   CREATE TABLE IF NOT EXISTS student (
       Name        STRING,
       facultyNo   INT,
       classNo     INT)
   USING csv
   OPTIONS (path 'obs://bucketName/filePath')
   PARTITIONED BY (facultyNo, classNo);

Example 3: Using CTAS to Create an OBS Non-Partitioned Table Using All or Part of the Data in the Source Table
--------------------------------------------------------------------------------------------------------------

Example description: Based on the OBS table **table1** created in :ref:`Example 1: Creating an OBS Non-Partitioned Table <dli_08_0076__section175482343414>`, use the CTAS syntax to copy data from **table1** to **table1_ctas**.

When using CTAS to create a table, you can ignore the syntax used to create the table being copied. This means that regardless of the syntax used to create **table1**, you can use the DataSource syntax to create **table1_ctas**.

In addition, in this example, the storage format of **table1** is **orc**, and the storage format of **table1_ctas** may be **parquet**. This means that the storage format of the table created by CTAS may be different from that of the original table.

Use the **SELECT** statement following the **AS** keyword to select required data and insert the data to **table1_ctas**.

The **SELECT** syntax is as follows: **SELECT <**\ *Column name* **> FROM <**\ *Table name* **> WHERE <**\ *Related filter criteria*\ **>**.

-  In this example, **SELECT \* FROM table1** is used. **\*** indicates that all columns are selected from **table1** and all data in **table1** is inserted into **table1_ctas**.

   ::

      CREATE TABLE IF NOT EXISTS table1_ctas
      USING parquet
      OPTIONS (path 'obs:// bucketName/filePath')
      AS
      SELECT  *
      FROM    table1;

-  To filter and insert data into **table1_ctas** in a customized way, you can use the following **SELECT** statement: **SELECT col_1 FROM table1 WHERE col_1 = 'Ann'**. This will allow you to select only **col_1** from **table1** and insert data into **table1_ctas** where the value equals **'Ann'**.

   ::

      CREATE TABLE IF NOT EXISTS table1_ctas
      USING parquet
      OPTIONS (path 'obs:// bucketName/filePath')
      AS
      SELECT  col_1
      FROM    table1
      WHERE   col_1 = 'Ann';

Example 4: Creating an OBS Non-Partitioned Table and Customizing the Data Type of a Column Field
------------------------------------------------------------------------------------------------

Example description: Create an OBS non-partitioned table named **table2**. You can customize the native data types of column fields based on service requirements.

-  **STRING**, **CHAR**, or **VARCHAR** can be used for text characters.
-  **TIMESTAMP** or **DATE** can be used for time characters.
-  **INT**, **SMALLINT/SHORT**, **BIGINT/LONG**, or **TINYINT** can be used for integer characters.
-  **FLOAT**, **DOUBLE**, or **DECIMAL** can be used for decimal calculation.
-  **BOOLEAN** can be used if only logical switches are involved.

For details, see "Data Types" > "Primitive Data Types".

::

   CREATE TABLE IF NOT EXISTS table2 (
       col_01  STRING,
       col_02  CHAR (2),
       col_03  VARCHAR (32),
       col_04  TIMESTAMP,
       col_05  DATE,
       col_06  INT,
       col_07  SMALLINT,
       col_08  BIGINT,
       col_09  TINYINT,
       col_10  FLOAT,
       col_11  DOUBLE,
       col_12  DECIMAL (10, 3),
       col_13  BOOLEAN
   )
   USING parquet
   OPTIONS (path 'obs://bucketName/filePath');

Example 5: Creating an OBS Partitioned Table and Customizing OPTIONS Parameters
-------------------------------------------------------------------------------

Example description: When creating an OBS table, you can customize property names and values. For details about OPTIONS parameters, see :ref:`Table 2 <dli_08_0076__dli_08_0076_en-us_topic_0114776170_table1376011233214>`.

In this example, an OBS partitioned table named **table3** is created and partitioned based on **col_2**. Configure **path**, **multiLevelDirEnable**, **dataDelegated**, and **compression** in **OPTIONS**.

-  **path**: OBS storage path. In this example, the value is **obs://**\ *bucketName*\ **/**\ *filePath*, where *bucketName* indicates the bucket name and *filePath* indicates the actual directory name.
-  In big data scenarios, you are advised to use the OBS parallel file system for storage.
-  **multiLevelDirEnable**: In this example, set this parameter to **true**, indicating that all files and subdirectories in the table path are read iteratively when the table is queried. If this parameter is not required, set it to **false** or leave it blank (the default value is **false**).
-  **dataDelegated**: In this example, set this parameter to **true**, indicating that all data in the path is deleted when a table or partition is deleted. If this parameter is not required, set it to **false** or leave it blank (the default value is **false**).
-  **compression**: If the created OBS table needs to be compressed, you can use the keyword **compression** to configure the compression format. In this example, the **zstd** compression format is used.

::

   CREATE TABLE IF NOT EXISTS table3 (
       col_1   STRING,
       col_2   int
   )
   USING parquet
   PARTITIONED BY (col_2)
   OPTIONS (
       path 'obs://bucketName/filePath',
       multiLeveldirenable = true,
       datadelegated = true,
       compression = 'zstd'
   );

Example 6: Creating an OBS Non-Partitioned Table and Customizing OPTIONS Parameters
-----------------------------------------------------------------------------------

Example description: A CSV table is a file format that uses commas to separate data values in plain text. It is commonly used for storing and sharing data, but it is not ideal for complex data types due to its lack of structured data concepts. So, when **file_format** is set to **csv**, more **OPTIONS** parameters can be configured. For details, see :ref:`Table 3 <dli_08_0076__dli_08_0076_en-us_topic_0114776170_table1876517231928>`.

In this example, a non-partitioned table named **table4** is created with a **csv** storage format, and additional **OPTIONS** parameters are used to constrain the data.

-  **delimiter**: data separator, indicating that commas (,) are used as separators between data
-  **quote**: quotation character, indicating that double quotation marks (") are used to quota the reference information in the data
-  **escape**: escape character, indicating that backslashes (\\) are used as the escape character for data storage
-  **multiLine**: This parameter is used to set the column data to be stored does not include carriage return or newline characters.
-  **dataFormat**: indicates that the date format of the **data** field in the CSV file is **yyyy-MM-dd**.
-  **timestamoFormat**: indicates that the timestamp format in the CSV file is **yyyy-MM-dd HH:mm:ss**.
-  **header**: indicates that the CSV table contains the table header information.
-  **nullValue**: indicates that **null** is set to indicate the null value in the CSV table.
-  **comment**: indicates that the CSV table uses a slash (/) to indicate the beginning of a comment.
-  **compression**: indicates that the CSV table is compressed in the **gzip**, **bzip2**, or **deflate** format. If compression is not required, set this parameter to **none**.
-  **encoding**: indicates that the table uses the **utf-8** encoding format. You can choose **utf-8**, **gb2312**, or **gbk** as needed. The default encoding format is **utf-8**.

::

   CREATE TABLE IF NOT EXISTS table4 (
       col_1 STRING,
       col_2 INT
   )
   USING csv
   OPTIONS (
       path 'obs://bucketName/filePath',
       delimiter       = ',',
       quote            = '#',
       escape           = '|',
       multiline        = false,
       dateFormat       = 'yyyy-MM-dd',
       timestampFormat  = 'yyyy-MM-dd HH:mm:ss',
       mode             = 'failfast',
       header           = true,
       nullValue        = 'null',
       comment          = '*',
       compression      = 'deflate',
       encoding         = 'utf - 8'
   );
