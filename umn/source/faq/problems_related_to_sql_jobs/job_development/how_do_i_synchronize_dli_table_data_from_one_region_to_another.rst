:original_name: dli_03_0072.html

.. _dli_03_0072:

How Do I Synchronize DLI Table Data from One Region to Another?
===============================================================

You can use the cross-region replication function of OBS. The procedure is as follows:

#. Export the DLI table data in region 1 to the user-defined OBS bucket. For details, see "Exporting Data from DLI to OBS" in *Data Lake Insight User Guide*.
#. Use the OBS cross-region replication function to replicate data to the OBS bucket in region 2. For details, see "Configuring Cross-Region Replication" in *Data Lake Insight User Guide*.
#. Import or use the corresponding data as required.
