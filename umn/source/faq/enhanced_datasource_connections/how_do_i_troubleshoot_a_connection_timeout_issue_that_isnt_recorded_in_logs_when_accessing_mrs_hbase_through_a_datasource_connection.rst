:original_name: dli_03_0080.html

.. _dli_03_0080:

How Do I Troubleshoot a Connection Timeout Issue That Isn't Recorded in Logs When Accessing MRS HBase Through a Datasource Connection?
======================================================================================================================================

If you have not added the cluster host information to the datasource connection, it can lead to KRB authentication failure, resulting in a connection timeout. In this case, there will not be any error message in the logs.

You are advised to reconfigure the host information and then try to access MRS HBase again.

Log in to the DLI console and choose **Datasource Connections** > **Enhanced**. On the **Enhanced** tab, select a connection and click **Modify Host**. In the displayed dialog box, enter the host information.

The information is in the format of *Host IP address* *Host name*\ **/**\ *Domain name*. Each piece of information is separated by a line break.

For details, see "Modifying Host Information" in *Data Lake Insight User Guide*.
