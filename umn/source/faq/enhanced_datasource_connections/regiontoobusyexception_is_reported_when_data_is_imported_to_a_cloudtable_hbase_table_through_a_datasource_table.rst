:original_name: dli_03_0252.html

.. _dli_03_0252:

RegionTooBusyException Is Reported When Data Is Imported to a CloudTable HBase Table Through a Datasource Table
===============================================================================================================

Symptom
-------

A datasource table was used to import data to a CloudTable HBase table. This HBase table contains a column family and a rowkey for 100 million simulating data records. The data volume is 9.76 GB. The job failed after 10 million data records were imported.

Cause Analysis
--------------

#. View driver error logs.
#. View executor error logs.
#. View task error logs.

The rowkey was poorly designed causing a large amount of traffic redirected to single or very few numbers of nodes.

Procedure
---------

#. Pre-partition the HBase.
#. Hash the rowkey.

Summary and Suggestions
-----------------------

Distribute data to different RegionServer. Add **distribute by rand()** to the end of the insert statement.
