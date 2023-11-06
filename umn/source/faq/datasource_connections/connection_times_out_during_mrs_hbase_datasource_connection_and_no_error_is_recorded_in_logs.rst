:original_name: dli_03_0080.html

.. _dli_03_0080:

Connection Times Out During MRS HBase Datasource Connection, and No Error Is Recorded in Logs
=============================================================================================

The cluster host information is not added to the datasource connection. As a result, the KRB authentication fails, the connection times out, and no error is recorded in logs. Configure the host information and try again.

On the **Enhanced** page, select the connection and click **Modify Host**. In the dialog box that is displayed, enter the host information. The format is **Host IP address Host name/Domain name**. Multiple records are separated by line breaks.

For details, see "Modifying the Host Information" in *Data Lake Insight User Guide*.
