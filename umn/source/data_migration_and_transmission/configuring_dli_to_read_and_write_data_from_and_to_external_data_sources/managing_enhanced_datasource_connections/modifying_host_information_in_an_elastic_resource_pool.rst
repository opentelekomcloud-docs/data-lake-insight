:original_name: dli_01_0013.html

.. _dli_01_0013:

Modifying Host Information in an Elastic Resource Pool
======================================================

Scenario
--------

Host information is the mapping between host IP addresses and domain names. After you configure host information, jobs can only use the configured domain names to access corresponding hosts. After a datasource connection is created, you can modify the host information.

When accessing the HBase cluster of MRS, you need to configure the host name (domain name) and IP address of the instance.

Constraints
-----------

You have obtained the MRS host information by referring to :ref:`How Do I Obtain MRS Host Information? <dli_01_0013__section3607172865810>`

.. _dli_01_0013__section6859216954:

Modifying Host Information
--------------------------

#. Log in to the DLI management console.

#. In the left navigation pane, choose **Datasource Connections**.

#. On the **Enhanced** tab page displayed, locate the enhanced datasource connection to be modified, click **More** in the **Operation** column, and select **Modify Host**.

#. In the **Modify Host** dialog box displayed, enter the obtained host information.

   Enter host information in the format of *Host IP address* *Host name*. Information about multiple hosts is separated by line breaks.

   Example:

   192.168.0.22 node-masterxxx1.com

   192.168.0.23 node-masterxxx2.com

   Obtain the MRS host information by referring to :ref:`How Do I Obtain MRS Host Information? <dli_01_0013__section3607172865810>`

#. Click **OK**.

.. _dli_01_0013__section3607172865810:

How Do I Obtain MRS Host Information?
-------------------------------------

-  **Method 1: View MRS host information on the management console.**

   To obtain the host name and IP address of an MRS cluster, for example, MRS 3.\ *x*, perform the following operations:

   #. Log in to the MRS management console.
   #. On the **Active Clusters** page displayed, click your desired cluster to access its details page.
   #. Click the **Components** tab.
   #. Click **ZooKeeper**.
   #. Click the **Instance** tab to view the corresponding service IP addresses. You can select any service IP address.
   #. Modify host information by referring to :ref:`Modifying Host Information <dli_01_0013__section6859216954>`.

   .. note::

      If the MRS cluster has multiple IP addresses, enter any service IP address when creating a datasource connection.

-  **Method 2: Obtain MRS host information from the /etc/hosts file on an MRS node.**

   #. Log in to any MRS node as user **root**.

   #. Run the following command to obtain MRS hosts information. Copy and save the information.

      **cat /etc/hosts**


      .. figure:: /_static/images/en-us_image_0000001586217017.png
         :alt: **Figure 1** Obtaining hosts information

         **Figure 1** Obtaining hosts information

   #. Modify host information by referring to :ref:`Modifying Host Information <dli_01_0013__section6859216954>`.
