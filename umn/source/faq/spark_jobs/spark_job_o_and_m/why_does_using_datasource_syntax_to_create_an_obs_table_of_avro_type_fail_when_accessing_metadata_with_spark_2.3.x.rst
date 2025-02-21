:original_name: dli_03_0275.html

.. _dli_03_0275:

Why Does Using DataSource Syntax to Create an OBS Table of Avro Type Fail When Accessing Metadata With Spark 2.3.x?
===================================================================================================================

Symptom
-------

I failed to use the DataSource syntax to create an OBS table in Avro format when selecting Spark to access metadata.


.. figure:: /_static/images/en-us_image_0000001594371725.png
   :alt: **Figure 1** Failed to create an OBS table in Avro format

   **Figure 1** Failed to create an OBS table in Avro format

Possible Causes
---------------

Spark 2.3.\ *x* does not support creating OBS tables in Avro format.

Solution
--------

When using the DataSource syntax to create an OBS table in Avro format, select Spark 2.4.\ *x* or later.
