:original_name: dli_03_0076.html

.. _dli_03_0076:

How Do I Use Python Scripts to Access the MySQL Database If the pymysql Module Is Missing from the Spark Job Results Stored in MySQL?
=====================================================================================================================================

#. If the pymysql module is missing, check whether the corresponding EGG package exists. If the package does not exist, upload the pyFile package on the **Package Management** page. The procedure is as follows:

   a. Upload the **egg** package to the specified OBS path.
   b. Log in to the DLI management console and choose **Data Management** > **Package Management**.
   c. On the **Package Management** page, click **Create Package** in the upper right corner to create a package.
   d. In the **Create Package** dialog, set the following parameters:

      -  Type: Select **PyFile**.
      -  **OBS Path**: Select the OBS path where the **egg** package is stored.
      -  Set **Group** and **Group Name** as you need.

   e. Click **OK**.
   f. On the Spark job editing page where the error is reported, choose the uploaded **egg** package from the **Python File Dependencies** drop-down list and run the Spark job again.

#. To interconnect PySpark jobs with MySQL, you need to create a datasource connection to enable the network between DLI and RDS.

   For how to create a datasource connection on the management console, see "Enhanced Datasource Connections" in *Data Lake Insight User Guide*.

   For how to call an API to create a datasource connection, see "Creating an Enhanced Datasource Connection" in *Data Lake Insight API Reference*.
