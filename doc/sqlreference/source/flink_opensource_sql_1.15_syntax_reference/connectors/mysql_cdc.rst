:original_name: dli_08_15059.html

.. _dli_08_15059:

MySQL CDC
=========

Function
--------

The MySQL CDC source table, that is, the MySQL streaming source table, reads all historical data in the database first and then smoothly switches data read to the Binlog to ensure data integrity.

.. table:: **Table 1** Supported types

   ===================== ============
   Type                  Description
   ===================== ============
   Supported Table Types Source table
   ===================== ============

Prerequisites
-------------

-  MySQL CDC requires MySQL 5.6, 5.7, or 8.0.\ *x*.

-  Fields in the **with** parameter can only be enclosed in single quotes.

-  An enhanced datasource connection has been created for DLI to connect to the MySQL database, so that you can configure security group rules as required.

-  Binlog is enabled for MySQL, and **binlog_row_image** is set to **FULL**.

-  A MySQL user has been created and granted the **SELECT**, **SHOW DATABASES**, **REPLICATION SLAVE**, and **REPLICATION CLIENT** permissions. Note: When the **scan.incremental.snapshot.enabled** parameter is enabled (enabled by default), there is no need to grant the reload permission.

   .. code-block::

      GRANT SELECT, SHOW DATABASES, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'user' IDENTIFIED BY 'password';

Caveats
-------

-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.
-  Storing authentication credentials such as usernames and passwords in code or plaintext poses significant security risks. It is recommended using DEW to manage credentials instead. Storing encrypted credentials in configuration files or environment variables and decrypting them when needed ensures security. For details, see .
-  Set a different SERVER ID for each reader.

   -  Each MySQL client used for reading Binlog should have a unique server ID to ensure that the MySQL server can distinguish between different clients and maintain their respective Binlog reading positions.
   -  Sharing the same server ID among different jobs may lead to reading data from incorrect Binlog positions, resulting in data inconsistency.
   -  You can assign a unique server ID to each source reader through SQL hints, such as using **SELECT \* FROM source_table /*+ OPTIONS('server-id'='5401-5404') \*/ ;** to allocate unique server IDs for four source readers.

-  Set up MySQL session timeouts.

   When an initial consistent snapshot is made for large databases, your established connection could time out while the tables are being read. You can prevent this behavior by configuring interactive_timeout and wait_timeout in your MySQL configuration file.

   -  **interactive_timeout**: The number of seconds the server waits for activity on an interactive connection before closing it. See `MySQL documentations <https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_interactive_timeout>`__.
   -  **wait_timeout**: The number of seconds the server waits for activity on a noninteractive connection before closing it. See `MySQL documentations <https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_wait_timeout>`__.

-  Precautions when using tables without primary keys:

   -  To use a table without primary keys, you must configure the **scan.incremental.snapshot.chunk.key-column** parameter and specify one non-null field.

   -  If there is an index in the table, use a column which is contained in the index in **scan.incremental.snapshot.chunk.key-column**. This will increase the speed of select statement.

      The processing semantics of a MySQL CDC table without primary keys is determined based on the behavior of the column that is specified by the **scan.incremental.snapshot.chunk.key-column**.

      -  If no update operation is performed on the specified column, the exactly-once semantics is ensured.
      -  If the update operation is performed on the specified column, only the at-least-once semantics is ensured. However, you can specify primary keys at downstream and perform the idempotence operation to ensure data correctness.

-  Watermarks cannot be defined for MySQL CDC source tables. For details about window aggregation, see :ref:`FAQ <dli_08_15059__section09412772314>`.

-  If you connect to a sink source that supports upsert, such as GaussDB(DWS) and MySQL, you need to define the primary key in the statement for creating the sink table. For details, see the printSink table creation statement in :ref:`Example <dli_08_15059__section2787228135313>`.

Features
--------

-  Incremental snapshot reading

   Incremental snapshot reading is a new mechanism to read snapshot of a table. Compared to the old snapshot mechanism, the incremental snapshot has many advantages, including:

   -  MySQL CDC Source can be parallel during snapshot reading
   -  MySQL CDC Source can perform checkpoints in the chunk granularity during snapshot reading
   -  MySQL CDC Source does not need to acquire global read lock (FLUSH TABLES WITH READ LOCK) before snapshot reading

   If you would like the source run in parallel, each parallel reader should have a unique server ID, so the **server-id** must be a range like **5400-6400**, and the range must be larger than the parallelism. During the incremental snapshot reading, the MySQL CDC Source firstly splits snapshot chunks (splits) by primary key of table, and then MySQL CDC Source assigns the chunks to multiple readers to read the data of snapshot chunk.

-  Lock-free

   The MySQL CDC source uses incremental snapshot algorithm, which avoids acquiring global read lock (FLUSH TABLES WITH READ LOCK) and thus does not need **RELOAD** permission.

-  Controlling parallelism

   Incremental snapshot reading provides the ability to read snapshot data parallelly.

-  Checkpoint

   Incremental snapshot reading provides the ability to perform checkpoint in chunk level. It resolves the checkpoint timeout problem in previous version with old snapshot reading mechanism.

Syntax
------

.. code-block::

   create table mySqlCdcSource (
     attr_name attr_type
     (',' attr_name attr_type)*
     (','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
   )
   with (
     'connector' = 'mysql-cdc',
     'hostname' = 'mysqlHostname',
     'username' = 'mysqlUsername',
     'password' = 'mysqlPassword',
     'database-name' = 'mysqlDatabaseName',
     'table-name' = 'mysqlTableName'
   );

Parameter Description
---------------------

.. table:: **Table 2** Source table parameters

   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                                  | Mandatory   | Default Value | Data Type   | Description                                                                                                                                                                                                                                                                                                                                                                                              |
   +============================================+=============+===============+=============+==========================================================================================================================================================================================================================================================================================================================================================================================================+
   | connector                                  | Yes         | None          | String      | Specify what connector to use, here should be **mysql-cdc**.                                                                                                                                                                                                                                                                                                                                             |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | hostname                                   | Yes         | None          | String      | IP address or hostname of the MySQL database server.                                                                                                                                                                                                                                                                                                                                                     |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | username                                   | Yes         | None          | String      | Name of the MySQL database to use when connecting to the MySQL database server.                                                                                                                                                                                                                                                                                                                          |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password                                   | Yes         | None          | String      | Password to use when connecting to the MySQL database server.                                                                                                                                                                                                                                                                                                                                            |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | database-name                              | Yes         | None          | String      | Database name of the MySQL server to monitor.                                                                                                                                                                                                                                                                                                                                                            |
   |                                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                          |
   |                                            |             |               |             | The **database-name** also supports regular expressions to monitor multiple tables match the regular expression.                                                                                                                                                                                                                                                                                         |
   |                                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                          |
   |                                            |             |               |             | -  Prefix matching: **^(test).\*** matches database names with the prefix **test**, for example, **test1** and **test2**.                                                                                                                                                                                                                                                                                |
   |                                            |             |               |             | -  Suffix matching: **.*[p$]** matches database names with the suffix **p**, for example, **cdcp** and **edcp**.                                                                                                                                                                                                                                                                                         |
   |                                            |             |               |             | -  Specific matching: **txc** matches a specific database name.                                                                                                                                                                                                                                                                                                                                          |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table-name                                 | Yes         | None          | String      | Table name of the MySQL database to monitor. The table-name also supports regular expressions to monitor multiple tables that satisfy the regular expressions.                                                                                                                                                                                                                                           |
   |                                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                          |
   |                                            |             |               |             | .. note::                                                                                                                                                                                                                                                                                                                                                                                                |
   |                                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                          |
   |                                            |             |               |             |    When the MySQL CDC connector regularly matches the table name, it will concat the database-name and table-name filled in by the user through the string \`\\\\.\` to form a full-path regular expression, and then use the regular expression to match the fully qualified name of the table in the MySQL database.                                                                                   |
   |                                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                          |
   |                                            |             |               |             |    -  Prefix matching: **^(test).\*** matches table names with the prefix **test**, for example, **test1** and **test2**.                                                                                                                                                                                                                                                                                |
   |                                            |             |               |             |    -  Suffix matching: **.*[p$]** matches table names with the suffix **p**, for example, **cdcp** and **edcp**.                                                                                                                                                                                                                                                                                         |
   |                                            |             |               |             |    -  Specific matching: **txc** matches a specific table name.                                                                                                                                                                                                                                                                                                                                          |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | port                                       | No          | 3306          | Integer     | Integer port number of the MySQL database server.                                                                                                                                                                                                                                                                                                                                                        |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | server-id                                  | No          | None          | String      | A numeric ID or a numeric ID range of this database client. The numeric ID syntax is like **5400**, the numeric ID range syntax is like **5400-5408**.                                                                                                                                                                                                                                                   |
   |                                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                          |
   |                                            |             |               |             | The numeric ID range syntax is recommended when **scan.incremental.snapshot.enabled** enabled.                                                                                                                                                                                                                                                                                                           |
   |                                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                          |
   |                                            |             |               |             | Every ID must be unique across all currently-running database processes in the MySQL cluster. This connector joins the MySQL cluster as another server (with this unique ID) so it can read the binlog. By default, a random number is generated between 5400 and 6400, though we recommend setting an explicit value.                                                                                   |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.incremental.snapshot.enabled          | No          | true          | Boolean     | Incremental snapshot is a new mechanism to read snapshot of a table. Compared to the old snapshot mechanism, the incremental snapshot has many advantages, including:                                                                                                                                                                                                                                    |
   |                                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                          |
   |                                            |             |               |             | -  MySQL CDC Source can be parallel during snapshot reading                                                                                                                                                                                                                                                                                                                                              |
   |                                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                          |
   |                                            |             |               |             | -  MySQL CDC Source can perform checkpoints in the chunk granularity during snapshot reading                                                                                                                                                                                                                                                                                                             |
   |                                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                          |
   |                                            |             |               |             | -  MySQL CDC Source does not need to acquire global read lock (FLUSH TABLES WITH READ LOCK) before snapshot reading                                                                                                                                                                                                                                                                                      |
   |                                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                          |
   |                                            |             |               |             |    If you would like the source run in parallel, each parallel reader should have a unique server ID, so the **server-id** must be a range like **5400-6400**, and the range must be larger than the parallelism.                                                                                                                                                                                        |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.incremental.snapshot.chunk.size       | No          | 8096          | Integer     | The chunk size (number of rows) of table snapshot, captured tables are split into multiple chunks when reading the snapshot of table.                                                                                                                                                                                                                                                                    |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.startup.mode                          | No          | initial       | String      | Optional startup mode for MySQL CDC consumer, valid enumerations are **initial**, **earliest-offset**, **latest-offset**, **specific-offset**, and **timestamp**.                                                                                                                                                                                                                                        |
   |                                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                          |
   |                                            |             |               |             | -  **initial** (default): Perform an initial snapshot on the monitored database tables upon first startup, and continue to read the latest binlog.                                                                                                                                                                                                                                                       |
   |                                            |             |               |             | -  **earliest-offset**: Skip snapshot phase and start reading binlog events from the earliest accessible binlog offset.                                                                                                                                                                                                                                                                                  |
   |                                            |             |               |             | -  **latest-offset**: Never to perform snapshot on the monitored database tables upon first startup, just read from the end of the binlog which means only have the changes since the connector was started.                                                                                                                                                                                             |
   |                                            |             |               |             | -  **specific-offset**: Skip snapshot phase and start reading binlog events from a specific offset. The offset could be specified with binlog filename and position, or a GTID set if GTID is enabled on server.                                                                                                                                                                                         |
   |                                            |             |               |             | -  **timestamp**: Skip snapshot phase and start reading binlog events from a specific timestamp.                                                                                                                                                                                                                                                                                                         |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.startup.specific-offset.file          | No          | None          | String      | Optional binlog file name used in case of **specific-offset** startup mode                                                                                                                                                                                                                                                                                                                               |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.startup.specific-offset.pos           | No          | None          | Long        | Optional binlog file position used in case of **specific-offset** startup mode                                                                                                                                                                                                                                                                                                                           |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.startup.specific-offset.gtid-set      | No          | None          | String      | Optional GTID set used in case of **specific-offset** startup mode                                                                                                                                                                                                                                                                                                                                       |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.startup.specific-offset.skip-events   | No          | None          | Long        | Optional number of events to skip after the specific starting offset                                                                                                                                                                                                                                                                                                                                     |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.startup.specific-offset.skip-rows     | No          | None          | Long        | Optional number of rows to skip after the specific starting offset                                                                                                                                                                                                                                                                                                                                       |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | server-time-zone                           | No          | None          | String      | Session time zone on the database server                                                                                                                                                                                                                                                                                                                                                                 |
   |                                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                          |
   |                                            |             |               |             | It controls how the TIMESTAMP type in MYSQL converted to STRING. If not set, then **ZoneId.systemDefault()** is used to determine the server time zone.                                                                                                                                                                                                                                                  |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium.min.row. count.to.stream.result   | No          | 1000          | Integer     | During a snapshot operation, the connector will query each included table to produce a read event for all rows in that table.                                                                                                                                                                                                                                                                            |
   |                                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                          |
   |                                            |             |               |             | This parameter determines whether the MySQL connection will pull all results for a table into memory (which is fast but requires large amounts of memory), or whether the results will instead be streamed (can be slower, but will work for very large tables). The value specifies the minimum number of rows a table must contain before the connector will stream results, and defaults to **1000**. |
   |                                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                          |
   |                                            |             |               |             | Set this parameter to **0** to skip all table size checks and always stream all results during a snapshot.                                                                                                                                                                                                                                                                                               |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connect.timeout                            | No          | 30s           | Duration    | The maximum time that the connector should wait after trying to connect to the MySQL database server before timing out.                                                                                                                                                                                                                                                                                  |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connect.max-retries                        | No          | 3             | Integer     | The max retry times that the connector should retry to build MySQL database server connection.                                                                                                                                                                                                                                                                                                           |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connection.pool.size                       | No          | 20            | Integer     | The connection pool size.                                                                                                                                                                                                                                                                                                                                                                                |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | jdbc.properties.\*                         | No          | None          | String      | Option to pass custom JDBC URL properties.                                                                                                                                                                                                                                                                                                                                                               |
   |                                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                          |
   |                                            |             |               |             | User can pass custom properties like **'jdbc.properties.useSSL' = 'false'**.                                                                                                                                                                                                                                                                                                                             |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | heartbeat.interval                         | No          | 30s           | Duration    | The interval of sending heartbeat event for tracing the latest available binlog offsets.                                                                                                                                                                                                                                                                                                                 |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | debezium.\*                                | No          | None          | String      | Pass-through Debezium's properties to Debezium Embedded Engine which is used to capture data changes from MySQL server.                                                                                                                                                                                                                                                                                  |
   |                                            |             |               |             |                                                                                                                                                                                                                                                                                                                                                                                                          |
   |                                            |             |               |             | For example: **'debezium.snapshot.mode' = 'never'**. See more about the `Debezium's MySQL Connector properties <https://debezium.io/documentation/reference/1.9/connectors/mysql.html#mysql-connector-properties>`__.                                                                                                                                                                                    |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.incremental.close-idle-reader.enabled | No          | false         | Boolean     | Whether to close idle readers at the end of the snapshot phase. This feature requires that **execution.checkpointing.checkpoints-after-tasks-finish.enabled** be set to **true**.                                                                                                                                                                                                                        |
   +--------------------------------------------+-------------+---------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Metadata
--------

The following format metadata can be exposed as read-only (VIRTUAL) columns in DDL.

.. table:: **Table 3** Metadata

   +-----------------------+---------------------------+----------------------------------------------------------------------------------------------------+
   | Key                   | Data Type                 | Description                                                                                        |
   +=======================+===========================+====================================================================================================+
   | table_name            | STRING NOT NULL           | Name of the table that contains the row.                                                           |
   +-----------------------+---------------------------+----------------------------------------------------------------------------------------------------+
   | database_name         | STRING NOT NULL           | Name of the database that contains the row.                                                        |
   +-----------------------+---------------------------+----------------------------------------------------------------------------------------------------+
   | op_ts                 | TIMESTAMP_LTZ(3) NOT NULL | It indicates the time that the change was made in the database.                                    |
   |                       |                           |                                                                                                    |
   |                       |                           | If the record is read from snapshot of the table instead of the binlog, the value is always **0**. |
   +-----------------------+---------------------------+----------------------------------------------------------------------------------------------------+

Data Type Mapping
-----------------

.. table:: **Table 4** Data type mapping

   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | MySQL Type                         | Flink SQL Type        | Remarks                                                                                                                               |
   +====================================+=======================+=======================================================================================================================================+
   | TINYINT                            | TINYINT               | ``-``                                                                                                                                 |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | SMALLINT                           | SMALLINT              | ``-``                                                                                                                                 |
   |                                    |                       |                                                                                                                                       |
   | TINYINT UNSIGNED                   |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | TINYINT UNSIGNED ZEROFILL          |                       |                                                                                                                                       |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | INT                                | INT                   | ``-``                                                                                                                                 |
   |                                    |                       |                                                                                                                                       |
   | MEDIUMINT                          |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | SMALLINT UNSIGNED                  |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | SMALLINT UNSIGNED ZEROFILL         |                       |                                                                                                                                       |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | BIGINT                             | BIGINT                | ``-``                                                                                                                                 |
   |                                    |                       |                                                                                                                                       |
   | INT UNSIGNED                       |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | INT UNSIGNED ZEROFILL              |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | MEDIUMINT UNSIGNED                 |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | MEDIUMINT UNSIGNED ZEROFILL        |                       |                                                                                                                                       |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | BIGINT UNSIGNED                    | DECIMAL(20, 0)        | ``-``                                                                                                                                 |
   |                                    |                       |                                                                                                                                       |
   | BIGINT UNSIGNED ZEROFILL           |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | SERIAL                             |                       |                                                                                                                                       |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | FLOAT                              | FLOAT                 | ``-``                                                                                                                                 |
   |                                    |                       |                                                                                                                                       |
   | FLOAT UNSIGNED                     |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | FLOAT UNSIGNED ZEROFILL            |                       |                                                                                                                                       |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | REAL                               | DOUBLE                | ``-``                                                                                                                                 |
   |                                    |                       |                                                                                                                                       |
   | REAL UNSIGNED                      |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | REAL UNSIGNED ZEROFILL             |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | DOUBLE                             |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | DOUBLE UNSIGNED                    |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | DOUBLE UNSIGNED ZEROFILL           |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | DOUBLE PRECISION                   |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | DOUBLE PRECISION UNSIGNED          |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | DOUBLE PRECISION UNSIGNED ZEROFILL |                       |                                                                                                                                       |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | NUMERIC(p, s)                      | DECIMAL(p, s)         | ``-``                                                                                                                                 |
   |                                    |                       |                                                                                                                                       |
   | NUMERIC(p, s) UNSIGNED             |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | NUMERIC(p, s) UNSIGNED ZEROFILL    |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | DECIMAL(p, s)                      |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | DECIMAL(p, s) UNSIGNE              |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | DDECIMAL(p, s) UNSIGNED ZEROFILL   |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | FIXED(p, s)                        |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | FIXED(p, s) UNSIGNED               |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | FIXED(p, s) UNSIGNED ZEROFILL      |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | where p <= 38                      |                       |                                                                                                                                       |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | NUMERIC(p, s)                      | STRING                | The precision for DECIMAL data type is up to 65 in MySQL, but the precision for DECIMAL is limited to 38 in Flink.                    |
   |                                    |                       |                                                                                                                                       |
   | NUMERIC(p, s) UNSIGNED             |                       | So if you define a decimal column whose precision is greater than 38, you should map it to STRING to avoid precision loss.            |
   |                                    |                       |                                                                                                                                       |
   | NUMERIC(p, s) UNSIGNED ZEROFILL    |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | DECIMAL(p, s)                      |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | DECIMAL(p, s) UNSIGNED             |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | DECIMAL(p, s) UNSIGNED ZEROFILL    |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | FIXED(p, s)                        |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | FIXED(p, s) UNSIGNED               |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | FIXED(p, s) UNSIGNED ZEROFILL      |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | where 38 < p <= 65                 |                       |                                                                                                                                       |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | BOOLEAN                            | BOOLEAN               | ``-``                                                                                                                                 |
   |                                    |                       |                                                                                                                                       |
   | TINYINT(1)                         |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | BIT(1)                             |                       |                                                                                                                                       |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | DATE                               | DATE                  | ``-``                                                                                                                                 |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | TIME [(p)]                         | TIME [(p)]            | ``-``                                                                                                                                 |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | TIMESTAMP [(p)]                    | TIMESTAMP [(p)]       | ``-``                                                                                                                                 |
   |                                    |                       |                                                                                                                                       |
   | DATETIME [(p)]                     |                       |                                                                                                                                       |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | CHAR(n)                            | CHAR(n)               | ``-``                                                                                                                                 |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | VARCHAR(n)                         | VARCHAR(n)            | ``-``                                                                                                                                 |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | BIT(n)                             | BINARY(⌈n/8⌉)         | ``-``                                                                                                                                 |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | BINARY(n)                          | BINARY(n)             | ``-``                                                                                                                                 |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | VARBINARY(N)                       | VARBINARY(N)          | ``-``                                                                                                                                 |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | TINYTEXT                           | STRING                | ``-``                                                                                                                                 |
   |                                    |                       |                                                                                                                                       |
   | TEXT                               |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | MEDIUMTEXT                         |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | LONGTEXT                           |                       |                                                                                                                                       |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | TINYBLOB                           | BYTES                 | Currently, for BLOB data type in MySQL, only the blob whose length is not greater than 2,147,483,647(2 \*\* 31 - 1) is supported.     |
   |                                    |                       |                                                                                                                                       |
   | BLOB                               |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | MEDIUMBLOB                         |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | LONGBLOB                           |                       |                                                                                                                                       |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | YEAR                               | INT                   | ``-``                                                                                                                                 |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | ENUM                               | STRING                | ``-``                                                                                                                                 |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | JSON                               | STRING                | The JSON data type will be converted into STRING with JSON format in Flink.                                                           |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | SET                                | ARRAY<STRING>         | As the SET data type in MySQL is a string object that can have zero or more values, it should always be mapped to an array of string. |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | GEOMETRY                           | STRING                | The spatial data types in MySQL will be converted into STRING with a fixed Json format.                                               |
   |                                    |                       |                                                                                                                                       |
   | POINT                              |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | LINESTRING                         |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | POLYGON                            |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | MULTIPOINT                         |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | MULTILINESTRING                    |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | MULTIPOLYGON                       |                       |                                                                                                                                       |
   |                                    |                       |                                                                                                                                       |
   | GEOMETRYCOLLECTION                 |                       |                                                                                                                                       |
   +------------------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_08_15059__section2787228135313:

Example
-------

This example demonstrates the use of MySQL-CDC to read data and metadata from an RDS for MySQL database in real-time and write it to a Print result table.

In this example, the RDS for MySQL database engine version is MySQL 5.7.33.

#. Create an enhanced datasource connection in the VPC and subnet where MySQL locates, and bind the connection to the required Flink elastic resource pool.

#. Set MySQL security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the MySQL address. If the connection passes the test, it is bound to the queue.

#. Create the **test** user in MySQL and grant them permissions. The SQL statements are as follows:

   .. code-block::

      CREATE USER 'test'@'%' IDENTIFIED BY 'xxx';
      GRANT SELECT, SHOW DATABASES, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'test';
      FLUSH PRIVILEGES;

#. Create a table named **cdc_order** in the Flink database of MySQL. The SQL statement is as follows (this statement requires the user to have the **CREATE** permission):

   .. code-block::

      CREATE TABLE `flink`.`cdc_order` (
          `order_id` VARCHAR(32) NOT NULL,
          `order_channel` VARCHAR(32) NULL,
          `order_time` VARCHAR(32) NULL,
          `pay_amount` DOUBLE  NULL,
          `real_pay` DOUBLE  NULL,
          `pay_time` VARCHAR(32) NULL,
          `user_id` VARCHAR(32) NULL,
          `user_name` VARCHAR(32) NULL,
          `area_id` VARCHAR(32) NULL,
          PRIMARY KEY (`order_id`)
      )   ENGINE = InnoDB
          DEFAULT CHARACTER SET = utf8mb4
          COLLATE = utf8mb4_general_ci;

#. Create a Flink OpenSource SQL job. Enter the following job script and submit the job.

   When you create a job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

   .. code-block::

      create table mysqlCdcSource(
        database_name STRING METADATA VIRTUAL,
        table_name STRING METADATA VIRTUAL,
        operation_ts TIMESTAMP_LTZ(3) METADATA FROM 'op_ts' VIRTUAL,
        order_id string,
        order_channel string,
        order_time string,
        pay_amount double,
        real_pay double,
        pay_time string,
        user_id string,
        user_name string,
        area_id STRING,
        primary key(order_id) not enforced
      ) with (
        'connector' = 'mysql-cdc',
        'hostname' = 'mysqlHostname',
        'username' = 'mysqlUsername',
        'password' = 'mysqlPassword',
        'database-name' = 'mysqlDatabaseName',
        'table-name' = 'mysqlTableName'
      );

      create table printSink(
        database_name string,
        table_name string,
        operation_ts TIMESTAMP_LTZ(3),
        order_id string,
        order_channel string,
        order_time string,
        pay_amount double,
        real_pay double,
        pay_time string,
        user_id string,
        user_name string,
        area_id STRING,
        primary key(order_id) not enforced
      ) with (
        'connector' = 'print'
      );
      insert into printSink select * from mysqlCdcSource;

#. Run the following commands in MySQL to insert test data (this statement requires the user to have the corresponding permission):

   .. code-block::

      insert into flink.cdc_order values
      ('202103241000000001','webShop','2021-03-24 10:00:00','100.00','100.00','2021-03-24 10:02:03','0001','Alice','330106'),
      ('202103241606060001','appShop','2021-03-24 16:06:06','200.00','180.00','2021-03-24 16:10:06','0001','Alice','330106');

      delete from flink.cdc_order  where order_channel = 'webShop';
      insert into flink.cdc_order values('202103251202020001','miniAppShop','2021-03-25 12:02:02','60.00','60.00','2021-03-25 12:03:00','0002','Bob','330110');

#. Perform the following operations to view the data result in the **taskmanager.out** file:

   a. Log in to the DLI management console. In the navigation pane on the left, choose **Job Management** > **Flink Jobs**.
   b. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
   c. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **taskmanager.out** file, and view result logs.

   The data result is as follows:

   .. code-block::

       +I[flink, cdc_order, 2023-11-10T07:41:12Z, 202103241000000001, webShop, 2021-03-24 10:00:00, 100.0, 100.0, 2021-03-24 10:02:03, 0001, Alice, 330106]
      +I[flink, cdc_order, 2023-11-10T07:41:12Z, 202103241606060001, appShop, 2021-03-24 16:06:06, 200.0, 180.0, 2021-03-24 16:10:06, 0001, Alice, 330106]
      -D[flink, cdc_order, 2023-11-10T07:41:59Z, 202103241000000001, webShop, 2021-03-24 10:00:00, 100.0, 100.0, 2021-03-24 10:02:03, 0001, Alice, 330106]
      +I[flink, cdc_order, 2023-11-10T07:42:00Z, 202103251202020001, miniAppShop, 2021-03-25 12:02:02, 60.0, 60.0, 2021-03-25 12:03:00, 0002, Bob, 330110]

.. _dli_08_15059__section09412772314:

FAQ
---

Q: How do I perform window aggregation if the MySQL CDC source table does not support definition of watermarks?

A: You can use the non-window aggregation method. That is, convert the time field into a window value, and then use **GROUP BY** to perform aggregation based on the window value.

For example, you can use the following script to collect statistics on the number of orders per minute (**order_time** indicates the order time, in the string format):

.. code-block::

   insert into printSink select DATE_FORMAT(order_time, 'yyyy-MM-dd HH:mm'), count(*) from mysqlCdcSource group by DATE_FORMAT(order_time, 'yyyy-MM-dd HH:mm');
