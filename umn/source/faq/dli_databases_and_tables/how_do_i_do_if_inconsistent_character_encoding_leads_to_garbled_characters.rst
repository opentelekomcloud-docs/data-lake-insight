:original_name: dli_03_0009.html

.. _dli_03_0009:

How Do I Do If Inconsistent Character Encoding Leads to Garbled Characters?
===========================================================================

To avoid garbled characters caused by inconsistent character encoding, you are advised to unify the encoding format of your data source when executing jobs in DLI.

DLI only supports UTF-8-encoded text, so your data needs to be encoded in UTF-8 when creating tables and importing data.

Before importing data into DLI, make sure that the source data file (such as CSV, JSON, etc.) is saved in UTF-8 encoding. If the data source is not encoded in UTF-8, convert it to UTF-8 encoding before importing.
