:original_name: dli_01_0489.html

.. _dli_01_0489:

Testing Address Connectivity
============================

It can be used to test the connectivity between the DLI queue and the peer IP address specified by the user in common scenarios, or the connectivity between the DLI queue and the peer IP address bound to the datasource connection in datasource connection scenarios. The operation is as follows:

#. On the **Queue Management** page, locate the row containing the target queue, click **More** in the **Operation** column, and select **Test Address Connectivity**.

#. On the **Test Address Connectivity** page, enter the address to be tested. The domain name and IP address are supported, and the port number can be specified.

#. Click **Test**.

   If the test address is reachable, a message is displayed on the page, indicating that the address is reachable.

   If the test address is unreachable, the system displays a message indicating that the address is unreachable. Check the network configurations and try again. Network configurations include the VPC peering and the datasource connection. Check whether they have been activated.
