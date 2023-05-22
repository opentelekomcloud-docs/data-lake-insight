:original_name: dli_01_0420.html

.. _dli_01_0420:

Importing Data to a DLI Table
=============================

Importing Data Using OBS
------------------------

On the DLI management console, you can import data stored on OBS to DLI tables from **Data Management > Databases and Tables > Table Management** and **SQL Editor** pages. For details, see :ref:`Importing Data to the Table <dli_01_0253>`.

Importing Data Using CDM
------------------------

Use the Cloud Data Migration (CDM) service to import data from OBS to DLI. You need to create a CDM queue first.

For details about how to create the queue, see "Migrating Data from OBS to DLI" in the *Cloud Data Migration User Guide*.

Pay attention to the following configurations:

-  The VPC to which the DLI account belongs is the same as the VPC of the CDM queue.
-  You need to create two links, including a DLI link and an OBS link.
-  The format of the file to be transmitted can be **CSV** or **JSON**.

Importing Data Using DIS
------------------------

Use the Data Ingestion Service (DIS) service to import data to DLI. You need to create a DIS stream.

For details, see "Creating a DIS Stream" in *Data Ingestion Service User Guide*.

When configuring the DIS stream, set the **Dump Destination** to **DLI** and select the database and table in DLI.
