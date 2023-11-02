:original_name: dli_08_0140.html

.. _dli_08_0140:

Data Permissions List
=====================

:ref:`Table 1 <dli_08_0140__en-us_topic_0114776234_en-us_topic_0093946808_table3615415710>` describes the SQL statement permission matrix in DLI in terms of permissions on databases, tables, and roles.

.. _dli_08_0140__en-us_topic_0114776234_en-us_topic_0093946808_table3615415710:

.. table:: **Table 1** Permission matrix

   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   | Category       | SQL statement                     | Permission                                                                | Description                                                    |
   +================+===================================+===========================================================================+================================================================+
   | Database       | DROP DATABASE db1                 | The **DROP_DATABASE** permission of **database.db1**                      | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | CREATE TABLE tb1(...)             | The **CREATE_TABLE** permission of **database.db1**                       | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | CREATE VIEW v1                    | The **CREATE_VIEW** permission of **database.db1**                        | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | EXPLAIN query                     | The **EXPLAIN** permission of **database.db1**                            | Depending on the permissions required by **query** statements. |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   | Table          | SHOW CREATE TABLE tb1             | The **SHOW_CREATE_TABLE** permission of **database.db1.tables.tb1**       | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | DESCRIBE [EXTENDED|FORMATTED] tb1 | The **DESCRIBE_TABLE** permission of **databases.db1.tables.tb1**         | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | DROP TABLE [IF EXISTS] tb1        | The **DROP_TABLE** permission of **database.db1.tables.tb1**              | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | SELECT \* FROM tb1                | The **SELECT** permission of **database.db1.tables.tb1**                  | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | SELECT count(``*``) FROM tb1      | The **SELECT** permission of **database.db1.tables.tb1**                  | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | SELECT \* FROM view1              | The **SELECT** permission of **database.db1.tables.view1**                | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | SELECT count(``*``) FROM view1    | The **SELECT** permission of **database.db1.tables.view1**                | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | LOAD DLI TABLE                    | The **INSERT_INTO_TABLE** permission of **database.db1.tables.tb1**       | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | INSERT INTO TABLE                 | The **INSERT_INTO_TABLE** permission of **database.db1.tables.tb1**       | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | INSERT OVERWRITE TABLE            | The **INSERT_OVERWRITE_TABLE** permission of **database.db1.tables.tb1**  | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | ALTER TABLE ADD COLUMNS           | The **ALTER_TABLE_ADD_COLUMNS** permission of **database.db1.tables.tb1** | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | ALTER TABLE RENAME                | The **ALTER_TABLE_RENAME** permission of **database.db1.tables.tb1**      | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   | ROLE&PRIVILEGE | CREATE ROLE                       | The **CREATE_ROLE** permission of **db**                                  | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | DROP ROLE                         | The **DROP_ROLE** permission of **db**                                    | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | SHOW ROLES                        | The **SHOW_ROLES** permission of **db**                                   | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | GRANT ROLES                       | The **GRANT_ROLE** permission of **db**                                   | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | REVOKE ROLES                      | The **REVOKE_ROLE** permission of **db**                                  | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | GRANT PRIVILEGE                   | The **GRANT_PRIVILEGE** permission of **db** or **table**                 | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | REVOKE PRIVILEGE                  | The **REVOKE_PRIVILEGE** permission of **db** or **table**                | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+
   |                | SHOW GRANT                        | The **SHOW_GRANT** permission of **db** or **table**                      | ``-``                                                          |
   +----------------+-----------------------------------+---------------------------------------------------------------------------+----------------------------------------------------------------+

For privilege granting or revocation on databases and tables, DLI supports the following permissions:

-  Permissions that can be assigned or revoked on databases are as follows:

   -  DROP_DATABASE (Deleting a database)
   -  CREATE_TABLE (Creating a table)
   -  CREATE_VIEW (Creating a view)
   -  EXPLAIN (Explaining a SQL statement as an execution plan)
   -  CREATE_ROLE (Creating a role)
   -  DROP_ROLE (Deleting a role)
   -  SHOW_ROLES (Displaying a role)
   -  GRANT_ROLE (Bounding a role)
   -  REVOKE_ROLE (Unbinding a role)
   -  DESCRIBE_TABLE (Describing a table)
   -  DROP_TABLE (Deleting a table)
   -  Select (Querying a table)
   -  INSERT_INTO_TABLE (Inserting)
   -  INSERT_OVERWRITE_TABLE (Overwriting)
   -  GRANT_PRIVILEGE (Granting permissions to a database)
   -  REVOKE_PRIVILEGE (Revoking permissions from a database)
   -  SHOW_PRIVILEGES (Viewing the database permissions of other users)
   -  ALTER_TABLE_ADD_PARTITION (Adding partitions to a partitioned table)
   -  ALTER_TABLE_DROP_PARTITION (Deleting partitions from a partitioned table)
   -  ALTER_TABLE_RENAME_PARTITION (Renaming table partitions)
   -  ALTER_TABLE_RECOVER_PARTITION (Restoring table partitions)
   -  ALTER_TABLE_SET_LOCATION (Setting the path of a partition)
   -  SHOW_PARTITIONS (Displaying all partitions)
   -  SHOW_CREATE_TABLE (Viewing table creation statements)

-  Permissions that can be assigned or revoked on tables are as follows:

   -  DESCRIBE_TABLE (Describing a table)
   -  DROP_TABLE (Deleting a table)
   -  Select (Querying a table)
   -  INSERT_INTO_TABLE (Inserting)
   -  INSERT_OVERWRITE_TABLE (Overwriting)
   -  GRANT_PRIVILEGE (Granting permissions to a table)
   -  REVOKE_PRIVILEGE (Revoking permissions from a table)
   -  SHOW_PRIVILEGES (Viewing the table permissions of other users)
   -  ALTER_TABLE_ADD_COLUMNS (Adding a column)
   -  ALTER_TABLE_RENAME (Renaming a table)
   -  ALTER_TABLE_ADD_PARTITION (Adding partitions to a partitioned table)
   -  ALTER_TABLE_DROP_PARTITION (Deleting partitions from a partitioned table)
   -  ALTER_TABLE_RENAME_PARTITION (Renaming table partitions)
   -  ALTER_TABLE_RECOVER_PARTITION (Restoring table partitions)
   -  ALTER_TABLE_SET_LOCATION (Setting the path of a partition)
   -  SHOW_PARTITIONS (Displaying all partitions)
   -  SHOW_CREATE_TABLE (Viewing table creation statements)
