:original_name: dli_01_0559.html

.. _dli_01_0559:

Creating a Password Datasource Authentication
=============================================

Scenario
--------

Create a password datasource authentication on the DLI console to store passwords of the GaussDB(DWS), RDS, DCS, and DDS data sources to DLI. This will allow you to access to the data sources without having to configure a username and password in SQL jobs.

Procedure
---------

#. Create a datasource authentication.

   a. Log in to the DLI management console.

   b. Choose **Datasource Connections**. On the page displayed, click **Datasource Authentication**.

   c. Click **Create**.

      Configure authentication parameters according to :ref:`Table 1 <dli_01_0559__table154674113817>`.

      .. _dli_01_0559__table154674113817:

      .. table:: **Table 1** Parameters

         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                        |
         +===================================+====================================================================================================================================+
         | Type                              | Select **Password**.                                                                                                               |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Authentication Certificate        | Name of the datasource authentication to be created.                                                                               |
         |                                   |                                                                                                                                    |
         |                                   | -  The name can contain only digits, letters, and underscores (_), but cannot contain only digits or start with an underscore (_). |
         |                                   | -  The name can contain a maximum of 128 characters.                                                                               |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Username                          | Username for accessing the data source.                                                                                            |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
         | Password                          | Password for accessing the data source.                                                                                            |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------+

#. Access the data source.

   When creating a data source, associate the data source with the created datasource authentication to access the data source.

   For details about how to create a table, see *Data Lake Insight Syntax Reference*.
