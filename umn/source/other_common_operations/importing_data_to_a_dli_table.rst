:original_name: dli_01_0420.html

.. _dli_01_0420:

Importing Data to a DLI Table
=============================

Importing Data Using OBS
------------------------

On the DLI management console, you can import data stored in OBS into DLI tables.

To import OBS data to a DLI table, either choose **Data Management** > **Databases and Tables** in the navigation pane on the left, locate a desired database, and click **Tables** in the **Operation** column, or choose **SQL Editor** in the navigation pane on the left.

For details, see :ref:`Importing Data to the Table <dli_01_0253>`.

Importing Data Using DIS
------------------------

Use the Data Ingestion Service (DIS) service to import data to DLI. You need to create a DIS stream.

For details, see "Creating a DIS Stream" in *Data Ingestion Service User Guide*.

When configuring the DIS stream, set the **Dump Destination** to **DLI** and select the database and table in DLI.
