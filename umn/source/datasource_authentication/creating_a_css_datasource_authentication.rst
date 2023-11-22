:original_name: dli_01_0427.html

.. _dli_01_0427:

Creating a CSS Datasource Authentication
========================================

Scenario
--------

Create a CSS datasource authentication on the DLI console to store the authentication information of the CSS security cluster to DLI. This will allow you to access to the CSS security cluster without having to configure a username and password in SQL jobs.

Create a datasource authentication for a CSS security cluster on the DLI console.

Notes
-----

A CSS security cluster has been created and has met the following conditions:

-  The cluster version is 6.5.4 or later.
-  The security mode has been enabled for the cluster.

Procedure
---------

#. Download the authentication credential of the CSS security cluster.

   a. Log in to the CSS management console and choose **Clusters** > **Elasticsearch**.
   b. On the **Clusters** page displayed, click the cluster name.
   c. On the **Cluster Information** page displayed, find the security mode and download the certificate of the CSS security cluster.

#. .. _dli_01_0427__li2302018145713:

   Upload the authentication credential to the OBS bucket.

#. Create a datasource authentication.

   a. Log in to the DLI management console.

   b. Choose **Datasource Connections**. On the page displayed, click **Datasource Authentication**.

   c. Click **Create**.

      Configure CSS authentication parameters according to :ref:`Table 1 <dli_01_0427__table1323455312373>`.

      .. _dli_01_0427__table1323455312373:

      .. table:: **Table 1** Parameters

         +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                                        |
         +===================================+====================================================================================================================================================+
         | Authentication Certificate        | Name of the datasource authentication information to be created.                                                                                   |
         |                                   |                                                                                                                                                    |
         |                                   | -  The name can contain only digits, letters, and underscores (_), but cannot contain only digits or start with an underscore (_).                 |
         |                                   | -  The length of the database name cannot exceed 128 characters.                                                                                   |
         |                                   | -  It is recommended that the name contain the CSS security cluster name to distinguish security authentication information of different clusters. |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
         | Type                              | Select **CSS**.                                                                                                                                    |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
         | Username                          | Username for logging in to the security cluster.                                                                                                   |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
         | Password                          | The password of the security cluster                                                                                                               |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
         | Certificate Path                  | Enter the OBS path to which the security certificate is uploaded, that is, the OBS bucket address in :ref:`2 <dli_01_0427__li2302018145713>`.      |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+

#. Create a table to access the CSS cluster.

   When creating a table, associate the table with the created datasource authentication to access the CSS cluster.

   For example, when using Spark SQL to create a table for accessing the CSS cluster, configure **es.certificate.name** to set the datasource authentication name and then connect to the CSS security cluster.
