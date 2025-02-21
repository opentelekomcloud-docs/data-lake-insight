:original_name: dli_03_0254.html

.. _dli_03_0254:

How Do I Do If an Insert Operation Failed After the Schema of the GaussDB(DWS) Source Table Is Updated?
=======================================================================================================

Symptom
-------

A datasource GaussDB(DWS) table and the datasource connection were created in DLI, and the schema of the source table in GaussDB(DWS) were updated. During the job execution, the schema of the source table failed to be updated, and the job failed.

Cause Analysis
--------------

When the insert operation is executed on the DLI datasource table, the GaussDB(DWS) source table is deleted and recreated. If the statement for creating the datasource table is not updated on DLI, the GaussDB(DWS) source table will fail to be updated.

Procedure
---------

Create a datasource table on DLI and add table creation configuration **truncate = true** to clear table data but not delete the table.

Summary and Suggestions
-----------------------

After the source table is updated, the corresponding datasource table must be updated too on DLI.
