:original_name: dli_09_0196.html

.. _dli_09_0196:

MRS Configuration
=================

.. _dli_09_0196__section18700849172311:

Configuring MRS Host Information in DLI Datasource Connection
-------------------------------------------------------------

#. Create a datasource connection on the DLI management console.

#. Add the **/etc/hosts** information of MRS cluster nodes to the **host** file of the DLI queue.

   For details, see section "Modifying the Host Information" in the *Data Lake Insight User Guide*.

.. _dli_09_0196__section12676527182715:

Completing Configurations for Enabling Kerberos Authentication
--------------------------------------------------------------

#. .. _dli_09_0196__li153021142712:

   Create a cluster with Kerberos authentication enabled by referring to section "Creating a Security Cluster and Logging In to MRS Manager" in . Add a user and grant permissions to the user by referring to section "Creating Roles and Users".

#. Use the user created in :ref:`1 <dli_09_0196__li153021142712>` for login authentication. For details, see . A human-machine user must change the password upon the first login.

#. Log in to Manager and choose **System**. In the navigation pane on the left, choose **Permission** > **User**, locate the row where the new user locates, click **More**, and select **Download Authentication Credential**. Save the downloaded package and decompress it to obtain the **keytab** and **krb5.conf** files.

Creating an MRS HBase Table
---------------------------

Before creating an MRS HBase table to be associated with the DLI table, ensure that the HBase table exists. The following provides example code to describe how to create an MRS HBase table:

#. Remotely log in to the ECS and use the HBase Shell command to view table information. In this command, **hbtest** indicates the name of the table to be queried.

   .. code-block::

      describe 'hbtest'

#. (Optional) If the HBase table does not exist, run the following command to create one:

   .. code-block::

      create 'hbtest', 'info', 'detail'

   In this command, **hbtest** indicates the table name, and other parameters indicate the column family names.

#. Configure the connection information. **TableName** corresponds to the name of the HBase table.
