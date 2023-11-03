:original_name: dli_08_0059.html

.. _dli_08_0059:

Complex Data Types
==================

Spark SQL supports complex data types, as shown in :ref:`Table 1 <dli_08_0059__en-us_topic_0093946794_tc03311896fc248a4883a320378f84a7a>`.

.. _dli_08_0059__en-us_topic_0093946794_tc03311896fc248a4883a320378f84a7a:

.. table:: **Table 1** Complex data types

   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
   | Data Type             | Description                                                                                                                                                                                                                                 | Syntax                                                                                                             |
   +=======================+=============================================================================================================================================================================================================================================+====================================================================================================================+
   | ARRAY                 | A set of ordered fields that construct an ARRAY with the specified values. The value can be of any type and the data type of all fields must be the same.                                                                                   | array(<value>,<value>[, ...])                                                                                      |
   |                       |                                                                                                                                                                                                                                             |                                                                                                                    |
   |                       |                                                                                                                                                                                                                                             | For details, see :ref:`Example of ARRAY <dli_08_0059__en-us_topic_0093946794_sdba49492924046f2a1ffb33968e73073>`.  |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
   | MAP                   | A group of unordered key/value pairs used to generate a MAP. The key must be native data type, but the value can be either native data type or complex data type. The type of the same MAP key, as well as the MAP value, must be the same. | map(K <key1>, V <value1>, K <key2>, V <value2>[, ...])                                                             |
   |                       |                                                                                                                                                                                                                                             |                                                                                                                    |
   |                       |                                                                                                                                                                                                                                             | For details, see :ref:`Example of Map <dli_08_0059__en-us_topic_0093946794_s2e73f4a839c94069b7811eac162dd4f5>`.    |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
   | STRUCT                | Indicates a group of named fields. The data types of the fields can be different.                                                                                                                                                           | struct(<value1>,<value2>[, ...])                                                                                   |
   |                       |                                                                                                                                                                                                                                             |                                                                                                                    |
   |                       |                                                                                                                                                                                                                                             | For details, see :ref:`Example of STRUCT <dli_08_0059__en-us_topic_0093946794_s955554408c884d2b85ddab9a3b86c6a4>`. |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+

Restrictions
------------

-  When a table containing fields of the complex data type is created, the storage format of this table cannot be CSV (txt).
-  If a table contains fields of the complex data type, data in CSV (txt) files cannot be imported to the table.
-  When creating a table of the MAP data type, you must specify the schema and do not support the **date**, **short**, and **timestamp** data types.
-  For the OBS table in JSON format, the key type of the MAP supports only the STRING type.
-  The key of the MAP type cannot be **NULL**. Therefore, the MAP key does not support implicit conversion between inserted data formats where NULL values are allowed. For example, the STRING type cannot be converted to other native types, the FLOAT type cannot be converted to the TIMESTAMP type, and other native types cannot be converted to the DECIMAL type.
-  Values of the **double** or **boolean** data type cannot be included in the **STRUCT** data type does not support the.

.. _dli_08_0059__en-us_topic_0093946794_sdba49492924046f2a1ffb33968e73073:

Example of ARRAY
----------------

Create an **array_test** table, set **id** to **ARRAY<INT>**, and **name** to **STRING**. After the table is created, insert test data into **array_test**. The procedure is as follows:

#. Create a table.

   **CREATE TABLE array_test(name STRING, id ARRAY < INT >) USING PARQUET;**

#. Run the following statements to insert test data:

   **INSERT INTO array_test VALUES ('test',array(1,2,3,4));**

   **INSERT INTO array_test VALUES ('test2',array(4,5,6,7))**

   **INSERT INTO array_test VALUES ('test3',array(7,8,9,0));**

#. Query the result.

   To query all data in the **array_test** table, run the following statement:

   **SELECT \* FROM** **array_test**;

   .. code-block::

      test3    [7,8,9,0]
      test2   [4,5,6,7]
      test    [1,2,3,4]

   To query the data of element **0** in the **id** array in the **array_test** table, run the following statement:

   **SELECT id[0] FROM** **array_test**;

   .. code-block::

      7
      4
      1

.. _dli_08_0059__en-us_topic_0093946794_s2e73f4a839c94069b7811eac162dd4f5:

Example of Map
--------------

Create the **map_test** table and set **score** to **map<STRING,INT>**. The key is of the **STRING** type and the value is of the **INT** type. After the table is created, insert test data to **map_test**. The procedure is as follows:

#. Create a table.

   **CREATE TABLE map_test(id STRING, score map<STRING,INT>) USING PARQUET;**

#. Run the following statements to insert test data:

   **INSERT INTO map_test VALUES ('test4',map('math',70,'chemistry',84));**

   **INSERT INTO map_test VALUES ('test5',map('math',85,'chemistry',97));**

   **INSERT INTO map_test VALUES ('test6',map('math',88,'chemistry',80));**

#. Query the result.

   To query all data in the **map_test** table, run the following statement:

   **SELECT \* FROM map_test;**

   .. code-block::

      test6    {"chemistry":80,"math":88}
      test5   {"chemistry":97,"math":85}
      test4   {"chemistry":84,"math":70}

   To query the math score in the **map_test** table, run the following statement:

   **SELECT id, score['Math'] FROM map_test;**

   .. code-block::

      test6    88
      test5   85
      test4   70

.. _dli_08_0059__en-us_topic_0093946794_s955554408c884d2b85ddab9a3b86c6a4:

Example of STRUCT
-----------------

Create a **struct_test** table and set **info** to the **STRUCT<name:STRING, age:INT>** data type (the field consists of **name** and **age**, where the type of **name** is **STRING** and **age** is **INT**). After the table is created, insert test data into the **struct_test** table. The procedure is as follows:

#. Create a table.

   **CREATE TABLE struct_test(id INT, info STRUCT<name:STRING,age:INT>) USING PARQUET;**

#. Run the following statements to insert test data:

   **INSERT INTO struct_test VALUES (8, struct('zhang',23));**

   **INSERT INTO struct_test VALUES (9, struct('li',25));**

   **INSERT INTO struct_test VALUES (10, struct('wang',26));**

#. Query the result.

   To query all data in the **struct_test** table, run the following statement:

   **SELECT \* FROM struct_test;**

   .. code-block::

      8    {"name":"zhang","age":23}
      10  {"name":"wang","age":26}
      9   {"name":"li","age":25}

   Query **name** and **age** in the **struct_test** table.

   **SELECT id,info.name,info.age FROM struct_test;**

   .. code-block::

      8    zhang   23
      10  wang    26
      9   li  25
