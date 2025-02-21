:original_name: dli_01_0489.html

.. _dli_01_0489:

Testing Address Connectivity
============================

DLI's address connectivity testing feature can be used to verify network connectivity between DLI queues and destination addresses.

This feature is typically utilized for reading and writing external data sources. Once a datasource connection is configured, the communication capability between the DLI queue and the bound peer address is verified.

Testing the Address Connectivity Between a Queue and the Data Source
--------------------------------------------------------------------

#. Log in to the DLI management console. In the navigation pane on the left, choose **Resources** > **Queue Management**.

#. On the **Queue Management** page, locate the row containing the target queue, click **More** in the **Operation** column, and select **Test Address Connectivity**.

#. On the **Test Address Connectivity** page, enter the address to be tested. The domain name and IP address are supported, and the port number can be specified.

   You can input the data source address in the following formats: IPv4 address; IPv4 address + Port number; Domain name; Domain name + Port number.

   · IPv4 address: 192.168.x.x

   · IPv4 + Port number: 192.168.x.x:8080

   · Domain name: domain-xxxxxx.com

   · Domain name + Port number: domain-xxxxxx.com:8080

#. Click **Test**.

   -  If the test address is reachable, a message is displayed on the page, indicating that the address is reachable.
   -  If the test address is unreachable, the system displays a message indicating that the address is unreachable. Check the network configurations and retry. Network configurations include the VPC peering and the datasource connection. Check whether they have been activated.

Related Operations
------------------

:ref:`Why Is a Datasource Connection Successfully Created But the Network Connectivity Test Fails? <dli_03_0179>`
