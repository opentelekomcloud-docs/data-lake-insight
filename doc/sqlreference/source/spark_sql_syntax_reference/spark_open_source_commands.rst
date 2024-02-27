:original_name: dli_08_0477.html

.. _dli_08_0477:

Spark Open Source Commands
==========================

This section describes the open source Spark SQL syntax supported by DLI. For details about the syntax, parameters, and examples, see `Spark SQL Syntax <https://spark.apache.org/docs/latest/sql-ref-syntax.html>`__.

.. table:: **Table 1** DLI Spark open source commands

   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Function                                              | Syntax Example                                                                                                          | DLI Spark 2.4.5 | DLI Spark 3.3.1 |
   +=======================================================+=========================================================================================================================+=================+=================+
   | Creating a database                                   | CREATE DATABASE testDB;                                                                                                 | Supported       | Supported       |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Creating a DLI table                                  | create table if not exists testDB.testTable1(                                                                           | Supported       | Supported       |
   |                                                       |                                                                                                                         |                 |                 |
   |                                                       | id int,                                                                                                                 |                 |                 |
   |                                                       |                                                                                                                         |                 |                 |
   |                                                       | age int,                                                                                                                |                 |                 |
   |                                                       |                                                                                                                         |                 |                 |
   |                                                       | money double                                                                                                            |                 |                 |
   |                                                       |                                                                                                                         |                 |                 |
   |                                                       | );                                                                                                                      |                 |                 |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Creating an OBS table                                 | create table if not exists testDB.testTable2(                                                                           | Supported       | Supported       |
   |                                                       |                                                                                                                         |                 |                 |
   |                                                       | id int,                                                                                                                 |                 |                 |
   |                                                       |                                                                                                                         |                 |                 |
   |                                                       | age int,                                                                                                                |                 |                 |
   |                                                       |                                                                                                                         |                 |                 |
   |                                                       | money double                                                                                                            |                 |                 |
   |                                                       |                                                                                                                         |                 |                 |
   |                                                       | )                                                                                                                       |                 |                 |
   |                                                       |                                                                                                                         |                 |                 |
   |                                                       | LOCATION 'obs://bucketName/filePath'                                                                                    |                 |                 |
   |                                                       |                                                                                                                         |                 |                 |
   |                                                       | partitioned by (dt string)                                                                                              |                 |                 |
   |                                                       |                                                                                                                         |                 |                 |
   |                                                       | row format delimited fields terminated by ','                                                                           |                 |                 |
   |                                                       |                                                                                                                         |                 |                 |
   |                                                       | STORED as TEXTFILE;                                                                                                     |                 |                 |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Inserting test data                                   | insert into table testDB.testTable2 values                                                                              | Supported       | Supported       |
   |                                                       |                                                                                                                         |                 |                 |
   |                                                       | (1, 18, 3.14, "20240101" ),                                                                                             |                 |                 |
   |                                                       |                                                                                                                         |                 |                 |
   |                                                       | (2, 18, 3.15, "20240102" );                                                                                             |                 |                 |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Changing database attributes                          | ALTER DATABASE testDB SET DBPROPERTIES ('Edited-by' = 'John');                                                          | Not supported   | Not supported   |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Changing the path for storing database files on OBS   | ALTER DATABASE testDB SET LOCATION 'obs://bucketName/filePath';                                                         | Not supported   | Not supported   |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Changing a table name                                 | ALTER TABLE testDB.testTable1 RENAME TO testDB.testTable101;                                                            | Supported       | Supported       |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Changing the name of a partition in a table           | ALTER TABLE testDB.testTable2 PARTITION ( dt='20240101') RENAME TO PARTITION ( dt='20240103');                          | Supported       | Supported       |
   |                                                       |                                                                                                                         |                 |                 |
   |                                                       | It only supports renaming partitions of tables stored in OBS, and the storage path of the table in OBS will not change. |                 |                 |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Adding a column                                       | ALTER TABLE testDB.testTable1 ADD COLUMNS (name string);                                                                | Supported       | Supported       |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Dropping a column                                     | ALTER TABLE testDB.testTable1 DROP columns (money);                                                                     | Not supported   | Not supported   |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Changing the name of a column                         | ALTER TABLE testDB.testTable1 RENAME COLUMN age TO years_of_age;                                                        | Not supported   | Not supported   |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Modifying column comments                             | ALTER TABLE testDB.testTable1 ALTER COLUMN age COMMENT "new comment";                                                   | Not supported   | Supported       |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Replacing a specified column                          | ALTER TABLE testDB.testTable1 REPLACE COLUMNS (name string, ID int COMMENT 'new comment');                              | Not supported   | Not supported   |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Customizing table attributes                          | ALTER TABLE testDB.testTable1 SET TBLPROPERTIES ('aaa' = 'bbb');                                                        | Supported       | Supported       |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Adding or modifying table comments                    | ALTER TABLE testDB.testTable1 SET TBLPROPERTIES ('comment' = 'test');                                                   | Supported       | Not supported   |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Changing the storage format of a table                | ALTER TABLE testDB.testTable1 SET fileformat csv;                                                                       | Not supported   | Not supported   |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Deleting a table attribute                            | ALTER TABLE testDB.testTable1 UNSET TBLPROPERTIES ('test');                                                             | Supported       | Supported       |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Deleting table comments                               | ALTER TABLE testDB.testTable1 UNSET TBLPROPERTIES ('comment');                                                          | Supported       | Supported       |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Displaying column information                         | DESCRIBE database_name.table_name col_name;                                                                             | Supported       | Supported       |
   |                                                       |                                                                                                                         |                 |                 |
   |                                                       | Example: **DESCRIBE testDB.testTable1 id;**                                                                             |                 |                 |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Displaying column information                         | DESCRIBE table_name table_name.col_name;                                                                                | Supported       | Supported       |
   |                                                       |                                                                                                                         |                 |                 |
   |                                                       | Example: **DESCRIBE testTable1 testTable1.id;**                                                                         |                 |                 |
   |                                                       |                                                                                                                         |                 |                 |
   |                                                       | It only supports viewing the column information for tables in the current database.                                     |                 |                 |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Returning metadata information of a query statement   | DESCRIBE QUERY SELECT age, sum(age) FROM testDB.testTable1 GROUP BY age;                                                | Not supported   | Supported       |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Returning metadata information of inserted data       | DESCRIBE QUERY VALUES(100, 10, 10000.20D) AS testTable1(id, age, money);                                                | Not supported   | Supported       |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Returning metadata information of a table             | DESCRIBE QUERY TABLE testTable1;                                                                                        | Not supported   | Supported       |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Returning metadata information of a column in a table | DESCRIBE FROM testTable1 SELECT age;                                                                                    | Not supported   | Supported       |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Returning the table creation statements for a table   | SHOW CREATE TABLE testDB.testTable1 AS SERDE;                                                                           | Not supported   | Supported       |
   |                                                       |                                                                                                                         |                 |                 |
   |                                                       | When executing this statement in Spark 3.3.1, it only applies to query the table creation statements for Hive tables.   |                 |                 |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
   | Returning the table creation statements for a table   | SHOW CREATE TABLE testDB.testTable1;                                                                                    | Supported       | Supported       |
   +-------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------+-----------------+
