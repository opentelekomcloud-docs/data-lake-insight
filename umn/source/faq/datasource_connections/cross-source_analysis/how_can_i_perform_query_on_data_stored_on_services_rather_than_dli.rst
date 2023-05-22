:original_name: dli_03_0011.html

.. _dli_03_0011:

How Can I Perform Query on Data Stored on Services Rather Than DLI?
===================================================================

To perform query on data stored on services rather than DLI, perform the following steps:

#. Assume that the data to be queried is stored on multiple services (for example, OBS) on cloud.
#. On the DLI management console, create a table and set the **Path** parameter to the save path of the target data, for example, path of an OBS bucket. The data is actually stored on OBS and data migration is not required.
#. On the DLI management console, edit SQL statements to query and analyze the data.
