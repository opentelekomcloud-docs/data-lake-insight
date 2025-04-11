:original_name: dli_03_0239.html

.. _dli_03_0239:

How Do I Do If I Encounter the "Incorrect string value" Error When Executing insert overwrite on a Datasource RDS Table?
========================================================================================================================

Symptom
-------

A datasource RDS table was created in the DataArts Studio, and the **insert overwrite** statement was executed to write data into RDS. **DLI.0999: BatchUpdateException: Incorrect string value: '\\xF0\\x9F\\x90\\xB3' for column 'robot_name' at row 1** was reported.

Cause Analysis
--------------

The data to be written contains emojis, which are encoded in the unit of four bytes. MySQL databases use the UTF-8 format, which encodes data in the unit of three bytes by default. In this case, an error occurs when the emoji data is inserted into to the MySQL database.

Possible causes are as follows:

-  A database coding error occurred.

Procedure
---------

Change the character set to **utf8mb4**.

#. Run the following SQL statement to change the database character set:

   .. code-block::

      ALTER DATABASE DATABASE_NAME DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;

#. Run the following SQL statement to change the table character set:

   .. code-block::

      ALTER TABLE TABLE_NAME DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;

#. Run the following SQL statement to change the character set for all fields in the table:

   .. code-block::

      ALTER TABLE TABLE_NAME CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
