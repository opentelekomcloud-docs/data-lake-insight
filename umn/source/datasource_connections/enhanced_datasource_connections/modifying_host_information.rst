:original_name: dli_01_0013.html

.. _dli_01_0013:

Modifying Host Information
==========================

.. _dli_01_0013__section636281512389:

Modifying Hosts Information
---------------------------

**Method 1: Copy hosts information in /etc/hosts of an MRS node.**

#. Log in to any MRS node as the **root** user.

#. .. _dli_01_0013__li5236194744818:

   Run the following command to obtain MRS hosts information. Copy and save the information.

   **cat /etc/hosts**


   .. figure:: /_static/images/en-us_image_0000001142261092.png
      :alt: **Figure 1** Obtaining hosts information

      **Figure 1** Obtaining hosts information

#. Log in to the DLI console, choose **Datasource Connections** > **Enhanced**. On the **Enhanced** tab page, select a connection and click **Modify Host**. In the displayed dialog box, paste the host information copied in :ref:`2 <dli_01_0013__li5236194744818>`. Click **OK**.

**Method 2: Log in to FusionInsight Manager to obtain MRS hosts information.**

#. Log in to FusionInsight Manager.

#. .. _dli_01_0013__li207601326501:

   On FusionInsight Manager, click **Hosts**. On the **Hosts** page, obtain the host names and service IP addresses of the MRS hosts.

#. Log in to the DLI console, choose **Datasource Connections** > **Enhanced**. On the **Enhanced** tab page, select a connection and click **Modify Host**. In the displayed dialog box, enter the host information. Click **OK**.

   The host information is in the format of *Service IP address* *Host name*. Specify the IP addresses and host names obtained in :ref:`2 <dli_01_0013__li207601326501>`, and separate multiple records by line breaks.

   For example:

   192.168.0.22 node-masterxxx1.com

   192.168.0.23 node-masterxxx2.com

.. note::

   -  After this configuration, all hosts information is overwritten.
   -  A host name or domain name can contain a maximum of 128 characters, including digits, letters, underscores (_), hyphens (-), and periods (.). It must start with a letter.
   -  The host information is *IP address Host name/Domain name* format. Separate multiple records by line breaks.
