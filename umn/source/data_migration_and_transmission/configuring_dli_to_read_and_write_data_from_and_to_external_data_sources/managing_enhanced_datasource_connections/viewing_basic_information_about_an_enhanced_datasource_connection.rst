:original_name: dli_01_0623.html

.. _dli_01_0623:

Viewing Basic Information About an Enhanced Datasource Connection
=================================================================

After creating an enhanced datasource connection, you can view and manage it on the management console.

This section describes how to view basic information about an enhanced datasource connection on the management console, including whether the enhanced datasource connection supports IPv6, host information, and more.

Procedure
---------

#. Log in to the DLI management console.

#. In the navigation pane on the left, choose **Datasource Connections**.

#. On the displayed **Enhanced** tab, locate the enhanced datasource connection whose basic information you want to view.

   -  In the upper right corner of the list page, click |image1| to customize the columns to display and set the rules for displaying the table content and the **Operation** column.
   -  In the search box above the list, you can filter the required enhanced datasource connection by name or tag.

#. Click |image2| to expand details about the enhanced datasource connection.

   You can view the following information:

   a. **IPv6 Support**: If you selected a subnet with IPv6 enabled when creating the enhanced datasource connection, then your enhanced datasource connection will support IPv6.
   b. **Host Information**: When accessing an MRS HBase cluster, you need to configure the host name (domain name) and the corresponding IP address of the instance. For details, see :ref:`Modifying Host Information in an Elastic Resource Pool <dli_01_0013>`.

.. |image1| image:: /_static/images/en-us_image_0000001891931040.png
.. |image2| image:: /_static/images/en-us_image_0000002132498398.png
