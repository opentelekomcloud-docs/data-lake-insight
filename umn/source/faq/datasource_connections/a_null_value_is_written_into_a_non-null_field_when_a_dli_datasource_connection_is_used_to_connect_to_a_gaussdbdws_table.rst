:original_name: dli_03_0253.html

.. _dli_03_0253:

A Null Value Is Written Into a Non-Null Field When a DLI Datasource Connection Is Used to Connect to a GaussDB(DWS) Table
=========================================================================================================================

Symptom
-------

A table was created on GaussDB(DWS) and then a datasource connection was created on DLI to read and write data. An error message was displayed during data writing, indicating that DLI was writing a null value to a non-null field of the table, and the job failed.

The error message is as follows:

.. code-block::

   DLI.0999: PSQLException: ERROR: dn_6009_6010: null value in column "ctr" violates not-null constraint
   Detail: Failing row contains (400070309, 9.00, 25, null, 2020-09-22, 2020-09-23 04:30:01.741).

Cause Analysis
--------------

#. The CIR field in the source table is of the **DOUBLE** type.


   .. figure:: /_static/images/en-us_image_0000001372601066.png
      :alt: **Figure 1** Creation statement of the source table

      **Figure 1** Creation statement of the source table

#. The field type in the target table is **DECIMAL(9,6)**.


   .. figure:: /_static/images/en-us_image_0000001422601037.png
      :alt: **Figure 2** Creation statement of the target table

      **Figure 2** Creation statement of the target table

#. View the source table data. The CTR value that causes the problem is **1675**, which exceed the precision (9 - 6 = 3 digits) of the **DECIMAL(9,6)** type. A null value was generated when the double value was converted to the decimal value, and the insert operation failed.

Procedure
---------

Change the precision of the decimal data defined in the target table.
