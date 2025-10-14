:original_name: dli_05_0044.html

.. _dli_05_0044:

Using Spark SQL Jobs to Analyze OBS Data
========================================

DLI allows you to use data stored on OBS. You can create OBS tables on DLI to access and process data in your OBS bucket.

This section describes how to create an OBS table on DLI, import data to the table, and insert and query table data.

Prerequisites
-------------

-  You have created an OBS bucket. For more information about OBS, see the Object Storage Service Console Operation Guide. In this example, the OBS bucket name is **dli-test-021**.

-  You have created a DLI SQL queue. For details, see Creating a Queue.

   **Note**: When you create the DLI queue, set **Type** to **For SQL**.

Preparations
------------

**Creating a Database on DLI**

#. Log in to the DLI management console and click **SQL Editor**. On the displayed page, set **Engine** to **spark** and **Queue** to the created SQL queue.

#. Enter the following statement in the SQL editing window to create the **testdb** database.

   .. code-block::

      create database testdb;

The following operations in this section must be performed for the **testdb** database.

DataSource and Hive Syntax for Creating an OBS Table on DLI
-----------------------------------------------------------

The main difference between DataSource syntax and Hive syntax lies in the range of table data storage formats supported and the number of partitions supported. For the key differences in creating OBS tables using these two syntax, refer to :ref:`Table 1 <dli_05_0044__en-us_topic_0000001242084772_table8559753103819>`.

.. _dli_05_0044__en-us_topic_0000001242084772_table8559753103819:

.. table:: **Table 1** Syntax differences

   +------------+--------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
   | Syntax     | Data Types                                             | Partitioning                                                                                                                                                                                                                                                            | Number of Partitions                                              |
   +============+========================================================+=========================================================================================================================================================================================================================================================================+===================================================================+
   | DataSource | ORC, PARQUET, JSON, CSV, and AVRO                      | You need to specify the partitioning column in both CREATE TABLE and PARTITIONED BY statements. For details, see :ref:`Creating a Single-Partition OBS Table Using DataSource Syntax <dli_05_0044__en-us_topic_0000001242084772_li1473310571410>`.                      | A maximum of 7,000 partitions can be created in a single table.   |
   +------------+--------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+
   | Hive       | TEXTFILE, AVRO, ORC, SEQUENCEFILE, RCFILE, and PARQUET | Do not specify the partitioning column in the CREATE TABLE statement. Specify the column name and data type in the PARTITIONED BY statement. For details, see :ref:`Creating an OBS Table Using Hive Syntax <dli_05_0044__en-us_topic_0000001242084772_li68154254017>`. | A maximum of 100,000 partitions can be created in a single table. |
   +------------+--------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------+

Creating an OBS Table Using the DataSource Syntax
-------------------------------------------------

The following describes how to create an OBS table for CSV files. The methods of creating OBS tables for other file formats are similar.

-  Create a non-partitioned OBS table.

   -  Specify an OBS file and create an OBS table for the CSV data.

      #. Create the **test.csv** file containing the following content and upload the **test.csv** file to the **root** directory of OBS bucket **dli-test-021**:

         .. code-block::

            Jordon,88,23
            Kim,87,25
            Henry,76,26

      #. Log in to the DLI management console and choose **SQL Editor** from the navigation pane on the left. In the SQL editing window, set **Engine** to **spark**, **Queue** to the SQL queue you have created, and **Database** to **testdb**. Run the following statement to create an OBS table:

         .. code-block::

            CREATE TABLE testcsvdatasource (name STRING, score DOUBLE, classNo INT
            ) USING csv OPTIONS (path "obs://dli-test-021/test.csv");

         .. caution::

            If you create an OBS table using a specified file, you cannot insert data to the table with DLI. The OBS file content is synchronized with the table data.

      #. Run the following statement to query data in the **testcsvdatasource** table.

         .. code-block::

            select * from testcsvdatasource;

      #. Open the **test.csv** file on the local PC, add **Aarn,98,20** to the file, and replace the original **test.csv** file in the OBS bucket.

         .. code-block::

            Jordon,88,23
            Kim,87,25
            Henry,76,26
            Aarn,98,20

      #. In the DLI **SQL Editor**, query the **testcsvdatasource** table for **Aarn,98,20**. The result is displayed.

         .. code-block::

            select * from testcsvdatasource;

   -  Specify an OBS directory and create an OBS table for CSV data.

      -  The specified OBS data directory does not contain files you want to import to the table.

         #. Create the file directory **data** in the **root** directory of the OBS bucket **dli-test-021**.

         #. Log in to the DLI management console and click **SQL Editor**. On the displayed page, set **Engine** to **spark**, **Queue** to the created SQL queue, and **Database** to **testdb**. Run the following statement to create OBS table **testcsvdata2source** in the **testdb** database on DLI:

            .. code-block::

               CREATE TABLE testcsvdata2source (name STRING, score DOUBLE, classNo INT) USING csv OPTIONS (path "obs://dli-test-021/data");

         #. Run the following statement to insert table data:

            .. code-block::

               insert into testcsvdata2source VALUES('Aarn','98','20');

         #. Run the following statement to query data in the **testcsvdata2source** table:

            .. code-block::

               select * from testcsvdata2source;

         #. Refresh the **obs://dli-test-021/data** directory of the OBS bucket and query the data. A CSV data file is generated, and the data is added to the file.

      -  The specified OBS data directory contains files you want to import to the table.

         #. Create file directory **data2** in the **root** directory of the OBS bucket **dli-test-021**. Create the **test.csv** file with the following content and upload the file to the **obs://dli-test-021/data2** directory:

            .. code-block::

               Jordon,88,23
               Kim,87,25
               Henry,76,26

         #. Log in to the DLI management console and click **SQL Editor**. On the displayed page, set **Engine** to **spark**, **Queue** to the created SQL queue, and **Database** to **testdb**. Run the following statement to create OBS table **testcsvdata3source** in the **testdb** database on DLI:

            .. code-block::

               CREATE TABLE testcsvdata3source (name STRING, score DOUBLE, classNo INT) USING csv OPTIONS (path "obs://dli-test-021/data2");

         #. Run the following statement to insert table data:

            .. code-block::

               insert into testcsvdata3source VALUES('Aarn','98','20');

         #. Run the following statement to query data in the **testcsvdata3source** table:

            .. code-block::

               select * from testcsvdata3source;

         #. Refresh the **obs://dli-test-021/data2** directory of the OBS bucket and query the data. A CSV data file is generated, and the data is added to the file.

-  Create an OBS partitioned table

   -  .. _dli_05_0044__en-us_topic_0000001242084772_li1473310571410:

      Create a single-partition OBS table

      #. Create file directory **data3** in the **root** directory of the OBS bucket **dli-test-021**.

      #. Log in to the DLI management console and click **SQL Editor**. On the displayed page, set **Engine** to **spark**, **Queue** to the created SQL queue, and **Database** to **testdb**. Run the following statement to create OBS table **testcsvdata4source** using data in the specified OBS directory **obs://dli-test-021/data3** and partition the table on the **classNo** column.

         .. code-block::

            CREATE TABLE testcsvdata4source (name STRING, score DOUBLE, classNo INT) USING csv OPTIONS (path "obs://dli-test-021/data3") PARTITIONED BY (classNo);

      3. Create the **classNo=25** directory in the **obs://dli-test-021/data3** directory of the OBS bucket. Create the **test.csv** file based on the following file content and upload the file to the **obs://dli-test-021/data3/classNo=25** directory of the OBS bucket.

         .. code-block::

            Jordon,88,25
            Kim,87,25
            Henry,76,25

      4. Run the following statement in the SQL editor to add the partition data to OBS table **testcsvdata4source**:

         .. code-block::

            ALTER TABLE
              testcsvdata4source
            ADD
              PARTITION (classNo = 25) LOCATION 'obs://dli-test-021/data3/classNo=25';

      5. Run the following statement to query data in the **classNo=25** partition of the **testcsvdata4source** table:

         .. code-block::

            select * from testcsvdata4source where classNo = 25;

      6. Run the following statement to insert the following data to the **testcsvdata4source** table:

         .. code-block::

            insert into testcsvdata4source VALUES('Aarn','98','25');
            insert into testcsvdata4source VALUES('Adam','68','24');

      7. Run the following statement to query data in the **classNo=25** and **classNo=24** partitions of the **testcsvdata4source** table:

         .. caution::

            When a partitioned table is queried using the where condition, the partition must be specified. Otherwise, the query fails and "DLI.0005: There should be at least one partition pruning predicate on partitioned table" is reported.

         .. code-block::

            select * from testcsvdata4source where classNo = 25;

         .. code-block::

            select * from testcsvdata4source where classNo = 24;

      8. In the **obs://dli-test-021/data3** directory of the OBS bucket, click the refresh button. Partition files are generated in the directory for storing the newly inserted table data.

   -  Create an OBS table partitioned on multiple columns.

      #. Create file directory **data4** in the **root** directory of the OBS bucket **dli-test-021**.

      #. Log in to the DLI management console and click **SQL Editor**. On the displayed page, set **Engine** to **spark**, **Queue** to the created SQL queue, and **Database** to **testdb**. Run the following statement to create OBS table **testcsvdata5source** using data in the specified OBS directory **obs://dli-test-021/data4** and partition the table on **classNo** and **dt** columns.

         .. code-block::

            CREATE TABLE testcsvdata5source (name STRING, score DOUBLE, classNo INT, dt varchar(16)) USING csv OPTIONS (path "obs://dli-test-021/data4") PARTITIONED BY (classNo,dt);

      #. Run the following statements to insert the following data into the **testcsvdata5source** table:

         .. code-block::

            insert into testcsvdata5source VALUES('Aarn','98','25','2021-07-27');
            insert into testcsvdata5source VALUES('Adam','68','25','2021-07-28');

      #. Run the following statement to query data in the **classNo** partition of the **testcsvdata5source** table:

         .. code-block::

            select * from testcsvdata5source where classNo = 25;

      #. Run the following statement to query data in the **dt** partition of the **testcsvdata5source** table:

         .. code-block::

            select * from testcsvdata5source where dt like '2021-07%';

      #. Refresh the **obs://dli-test-021/data4** directory of the OBS bucket. The following data files are generated:

         -  File directory 1: **obs://dli-test-021/data4/**\ *xxxxxx*\ **/classNo=25/dt=2021-07-27**
         -  File directory 2: **obs://dli-test-021/data4/**\ *xxxxxx*\ **/classNo=25/dt=2021-07-28**

      #. Create the partition directory **classNo=24** in **obs://dli-test-021/data4**, and then create the subdirectory **dt=2021-07-29** in **classNo=24**. Create the **test.csv** file using the following file content and upload the file to the **obs://dli-test-021/data4/classNo=24/dt=2021-07-29** directory.

         .. code-block::

            Jordon,88,24,2021-07-29
            Kim,87,24,2021-07-29
            Henry,76,24,2021-07-29

      #. Run the following statement in the SQL editor to add the partition data to OBS table **testcsvdata5source**:

         .. code-block::

            ALTER TABLE
              testcsvdata5source
            ADD
              PARTITION (classNo = 24,dt='2021-07-29') LOCATION 'obs://dli-test-021/data4/classNo=24/dt=2021-07-29';

      #. Run the following statement to query data in the **classNo** partition of the **testcsvdata5source** table:

         .. code-block::

            select * from testcsvdata5source where classNo = 24;

      #. Run the following statement to query all data in July 2021 in the **dt** partition:

         .. code-block::

            select * from testcsvdata5source where dt like '2021-07%';

Creating an OBS Table Using Hive Syntax
---------------------------------------

The following describes how to create an OBS table for TEXTFILE files. The methods of creating OBS tables for other file formats are similar.

-  Create a non-partitioned OBS table.

   #. Create file directory **data5** in the **root** directory of the OBS bucket **dli-test-021**. Create the **test.txt** file based on the following file content and upload the file to the **obs://dli-test-021/data5** directory:

      .. code-block::

         Jordon,88,23
         Kim,87,25
         Henry,76,26

   #. Log in to the DLI management console and click **SQL Editor**. On the displayed page, set **Engine** to **spark**, **Queue** to the created SQL queue, and **Database** to **testdb**. Run the following Hive statement to create an OBS table using data in **obs://dli-test-021/data5/test.txt** and set the row data delimiter to commas (,):

      .. code-block::

         CREATE TABLE hiveobstable (name STRING, score DOUBLE, classNo INT) STORED AS TEXTFILE LOCATION 'obs://dli-test-021/data5' ROW FORMAT DELIMITED FIELDS TERMINATED BY ',';

      .. note::

         **ROW FORMAT DELIMITED FIELDS TERMINATED BY ','** indicates that records are separated by commas (,).

   #. Run the following statement to query data in the **hiveobstable** table:

      .. code-block::

         select * from hiveobstable;

   #. Run the following statements to insert data into the table:

      .. code-block::

         insert into hiveobstable VALUES('Aarn','98','25');
         insert into hiveobstable VALUES('Adam','68','25');

   #. Run the following statement to query data in the table to verify that the data has been inserted:

      .. code-block::

         select * from hiveobstable;

   #. In the **obs://dli-test-021/data5** directory, refresh the page and query the data. Two files are generated containing the newly inserted data.

   **Create an OBS Table Containing Data of Multiple Formats**

   #. Create file directory **data6** in the **root** directory of the OBS bucket **dli-test-021**. Create the **test.txt** file based on the following file content and upload the file to the **obs://dli-test-021/data6** directory:

      .. code-block::

         Jordon,88-22,23:21
         Kim,87-22,25:22
         Henry,76-22,26:23

   #. Log in to the DLI management console and click **SQL Editor**. On the displayed page, set **Engine** to **spark**, **Queue** to the created SQL queue, and **Database** to **testdb**. Run the following Hive statement to create an OBS table using data stored in **obs://dli-test-021/data6**.

      .. code-block::

         CREATE TABLE hiveobstable2 (name STRING, hobbies ARRAY<string>, address map<string,string>) STORED AS TEXTFILE LOCATION 'obs://dli-test-021/data6'
         ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
         COLLECTION ITEMS TERMINATED BY '-'
         MAP KEYS TERMINATED BY ':';

      .. note::

         -  **ROW FORMAT DELIMITED FIELDS TERMINATED BY ','** indicates that records are separated by commas (,).
         -  **COLLECTION ITEMS TERMINATED BY '-'** indicates that the second column **hobbies** is in array format. Elements are separated by hyphens (-).
         -  **MAP KEYS TERMINATED BY ':'** indicates that the **address** column is in the key-value format. Key-value pairs are separated by colons (:).

   #. Run the following statement to query data in the **hiveobstable2** table:

      .. code-block::

         select * from hiveobstable2;

-  .. _dli_05_0044__en-us_topic_0000001242084772_li68154254017:

   Create a partitioned OBS table.

   #. Create file directory **data7** in the **root** directory of the OBS bucket **dli-test-021**.

   #. Log in to the DLI management console and click **SQL Editor**. On the displayed page, set **Engine** to **spark**, **Queue** to the created SQL queue, and **Database** to **testdb**. Run the following statement to create an OBS table using data stored in **obs://dli-test-021/data7** and partition the table on the **classNo** column:

      .. code-block::

         CREATE TABLE IF NOT EXISTS hiveobstable3(name STRING, score DOUBLE) PARTITIONED BY (classNo INT) STORED AS TEXTFILE LOCATION 'obs://dli-test-021/data7' ROW FORMAT DELIMITED FIELDS TERMINATED BY ',';

      .. caution::

         You can specify the partition key in the **PARTITIONED BY** statement. Do not specify the partition key in the **CREATE TABLE IF NOT EXISTS** statement. The following is an incorrect example:

         CREATE TABLE IF NOT EXISTS hiveobstable3(name STRING, score DOUBLE, classNo INT) PARTITIONED BY (classNo) STORED AS TEXTFILE LOCATION 'obs://dli-test-021/data7';

   #. Run the following statements to insert data into the table:

      .. code-block::

         insert into hiveobstable3 VALUES('Aarn','98','25');
         insert into hiveobstable3 VALUES('Adam','68','25');

   #. Run the following statement to query data in the table:

      .. code-block::

         select * from hiveobstable3 where classNo = 25;

   #. Refresh the **obs://dli-test-021/data7** directory. A new partition directory **classno=25** is generated containing the newly inserted table data.

   #. Create partition directory **classno=24** in the **obs://dli-test-021/data7** directory. Create the **test.txt** file using the following file content and upload the file to the **obs://dli-test-021/data7/classno=24** directory:

      .. code-block::

         Jordon,88,24
         Kim,87,24
         Henry,76,24

   #. Run the following statement in the SQL editor to add the partition data to OBS table **hiveobstable3**:

      .. code-block::

         ALTER TABLE
           hiveobstable3
         ADD
           PARTITION (classNo = 24) LOCATION 'obs://dli-test-021/data7/classNo=24';

   #. Run the following statement to query data in the **hiveobstable3** table:

      .. code-block::

         select * from hiveobstable3 where classNo = 24;

FAQs
----

-  **Q1**: What should I do if the following error is reported when the OBS partition table is queried?

   .. code-block::

      DLI.0005: There should be at least one partition pruning predicate on partitioned table `xxxx`.`xxxx`.;

   **Cause**: The partition key is not specified in the query statement of a partitioned table.

   **Solution**: Ensure that the where condition contains at least one partition key.

-  **Q2**: What should I do if "DLI.0007: The output path is a file, don't support INSERT...SELECT error" is reported when I use a DataSource statement to insert data in a specified OBS directory into an OBS table and the execution fails?

   The statement is similar to the following:

   .. code-block::

      CREATE TABLE testcsvdatasource (name string, id int) USING csv OPTIONS (path "obs://dli-test-021/data/test.csv");

   **Cause**: Data cannot be inserted if a specific file is used in the table creation statement. For example, the OBS file **obs://dli-test-021/data/test.csv** is used in the preceding example.

   **Solution**: Replace the OBS file to the file directory. You can insert data using the INSERT statement. The preceding example statement can be modified as follows:

   .. code-block::

      CREATE TABLE testcsvdatasource (name string, id int) USING csv OPTIONS (path "obs://dli-test-021/data");

-  **Q3**: What should I do if the syntax of a Hive statement used to create a partitioned OBS table is incorrect? For example, the following statement creates an OBS table partitioned on **classNo**:

   .. code-block::

      CREATE TABLE IF NOT EXISTS testtable(name STRING, score DOUBLE, classNo INT) PARTITIONED BY (classNo) STORED AS TEXTFILE LOCATION 'obs://dli-test-021/data7';

   **Cause**: Do not specify the partition key in the list following the table name. Specify the partition key in the **PARTITIONED BY** statement.

   **Solution**: Specify the partition key in **PARTITIONED BY**. For example:

   .. code-block::

      CREATE TABLE IF NOT EXISTS testtable(name STRING, score DOUBLE) PARTITIONED BY (classNo INT) STORED AS TEXTFILE LOCATION 'obs://dli-test-021/data7';
