:original_name: dli_03_0231.html

.. _dli_03_0231:

How Can I Resolve Data Inconsistencies When Importing Data from DLI to OBS?
===========================================================================

Symptom
-------

When DLI is used to insert data into an OBS temporary table, only part of data is imported.

Possible Causes
---------------

Possible causes are as follows:

-  The amount of data read during job execution is incorrect.
-  The data volume is incorrectly verified.

Run a query statement to check whether the amount of imported data is correct.

If OBS limits the number of files to be stored, add **DISTRIBUTE BY number** to the end of the insert statement.

For example, if **DISTRIBUTE BY 1** is added to the end of the insert statement, multiple files generated by multiple tasks can be inserted into one file.

Procedure
---------

#. On the DLI management console, check whether the number of results in the SQL job details is correct. The check result shows that the amount of data is correct.

#. Check whether the method to verify the data volume is correct. Perform the following steps to verify the data amount:

   a. Download the data file from OBS.
   b. Use the text editor to open the data file. The data volume is less than the expected volume.

   If you used this method, you can verify that the text editor cannot read all the data.

   Run the query statement to view the amount of data import into the OBS bucket. The query result indicates that all the data is imported.

   This issue is caused by incorrect verification of the data volume.
