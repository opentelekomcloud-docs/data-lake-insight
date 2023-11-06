:original_name: dli_03_0179.html

.. _dli_03_0179:

How Do I Do if the Datasource Connection Is Created But the Network Connectivity Test Fails?
============================================================================================

Description
-----------

A datasource connection is created and bound to a queue. The connectivity test fails and the following error information is displayed:

.. code-block::

   failed to connect to specified address

Fault Locating
--------------

The issues here are described in order of how likely they are to occur.

Troubleshoot the issue by ruling out the causes described here, one by one.

-  :ref:`Check Whether a Port Number Is Added to the End of the Domain Name or IP Address <dli_03_0179__en-us_topic_0000001250602862_section1598718651714>`
-  :ref:`Check Whether the Information of the Peer VPC and Subnet Are Correct. <dli_03_0179__en-us_topic_0000001250602862_section1248131717217>`
-  :ref:`Check Whether the CIDR Block of the Queue Overlaps That of the Data Source <dli_03_0179__en-us_topic_0000001250602862_section11573112152719>`
-  :ref:`Check Whether the VPC Administrator Permission Is Granted to DLI <dli_03_0179__en-us_topic_0000001250602862_section10740107124612>`
-  :ref:`Check Whether the Destination Security Group Allows Access from the CIDR Block of the Queue <dli_03_0179__en-us_topic_0000001250602862_section96661220175613>`
-  :ref:`Check the Route Information of the VPC Peering Connection Corresponding to an Enhanced Datasource Connection <dli_03_0179__en-us_topic_0000001250602862_section1510101713111>`
-  :ref:`Check Whether VPC Network ACL Rules Are Configured to Restrict Network Access <dli_03_0179__en-us_topic_0000001250602862_section1789710470256>`

.. _dli_03_0179__en-us_topic_0000001250602862_section1598718651714:

Check Whether a Port Number Is Added to the End of the Domain Name or IP Address
--------------------------------------------------------------------------------

The port number is required for the connectivity test.

The following example tests the connectivity between a queue and a specified RDS DB instance. The RDS DB instance uses port 3306.

The following figure shows how you should specify the IP address.

.. _dli_03_0179__en-us_topic_0000001250602862_section1248131717217:

Check Whether the Information of the Peer VPC and Subnet Are Correct.
---------------------------------------------------------------------

When you create an enhanced datasource connection, you need to specify the peer VPC and subnet.

For example, to test the connectivity between a queue and a specified RDS DB instance, you need to specify the RDS VPC and subnet information.

.. _dli_03_0179__en-us_topic_0000001250602862_section11573112152719:

Check Whether the CIDR Block of the Queue Overlaps That of the Data Source
--------------------------------------------------------------------------

The CIDR block of the DLI queue bound with a datasource connection cannot overlap the CIDR block of the data source.

You can check whether they overlap by viewing the connection logs.

CIDR block conflicts of queue A and queue B. In this example, queue B is bound to an enhanced datasource connection to data source C. Therefore, a message is displayed, indicating that the network segment of queue A conflicts with that of data source C. As a result, a new enhanced datasource connection cannot be established.

**Solution**: Modify the CIDR block of the queue or create another queue.

Planing the CIDR blocks for your queues helps you to avoid this problem.

.. _dli_03_0179__en-us_topic_0000001250602862_section10740107124612:

Check Whether the VPC Administrator Permission Is Granted to DLI
----------------------------------------------------------------

View the connection logs to check whether there is the required permission.

:ref:`Figure 1 <dli_03_0179__en-us_topic_0000001250602862_fig10740878465>` and :ref:`Figure 2 <dli_03_0179__en-us_topic_0000001250602862_fig19762856104817>` show the logs when subnet ID and route ID of the destination cannot be obtained because there is no permission.

**Solution**: Grant DLI the VPC Administrator permission and cancel the IAM ReadOnlyAccess authorization.

.. _dli_03_0179__en-us_topic_0000001250602862_fig10740878465:

.. figure:: /_static/images/en-us_image_0000001377545298.png
   :alt: **Figure 1** Viewing connection logs

   **Figure 1** Viewing connection logs

.. _dli_03_0179__en-us_topic_0000001250602862_fig19762856104817:

.. figure:: /_static/images/en-us_image_0000001427744557.png
   :alt: **Figure 2** Viewing connection logs

   **Figure 2** Viewing connection logs

.. _dli_03_0179__en-us_topic_0000001250602862_section96661220175613:

Check Whether the Destination Security Group Allows Access from the CIDR Block of the Queue
-------------------------------------------------------------------------------------------

To connect to Kafka, GaussDB(DWS), and RDS instances, add security group rules for the DLI CIDR block to the security group where the instances belong. For example, to connect a queue to RDS, perform the following operations:

#. Log in to the DLI console, choose **Resources** > **Queue Management** in the navigation pane on the left. On the displayed page, select the target queue, and click |image1| to expand the row containing the target queue to view its CIDR block.
#. On the **Instance Management** page of the RDS console, click the instance name. In the **Connection Information** area, locate **Database Port** to obtain the port number of the RDS DB instance.
#. In the **Connection Information** area locate the **Security Group** and click the group name to switch to the security group management page. Select the **Inbound Rules** tab and click **Add Rule**. Set the priority to 1, protocol to TCP, port to the database port number, and source to the CIDR block of the DLI queue. Click **OK**.

.. _dli_03_0179__en-us_topic_0000001250602862_section1510101713111:

Check the Route Information of the VPC Peering Connection Corresponding to an Enhanced Datasource Connection
------------------------------------------------------------------------------------------------------------

Check the routing table of the VPC peering connection corresponding to the enhanced datasource connection. Check whether the CIDR block of the queue overlaps other CIDR blocks in the routing table. If it does, the forwarding may be incorrect.

#. Obtain the ID of the VPC peering connection created for the enhanced datasource connection.
#. View the information about the VPC peering connection on the VPC console.
#. View the route table information of the VPC corresponding to the queue.

.. _dli_03_0179__en-us_topic_0000001250602862_section1789710470256:

Check Whether VPC Network ACL Rules Are Configured to Restrict Network Access
-----------------------------------------------------------------------------

Check whether an ACL is configured for the subnet corresponding to the datasource connection and whether the ACL rules restrict network access.

For example, if you set a CIDR block whose security group rule allows access from a queue and set a network ACL rule to deny access from that CIDR block, the security group rule does not take effect.

.. |image1| image:: /_static/images/en-us_image_0000001428187933.png
