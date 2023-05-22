:original_name: dli_03_0238.html

.. _dli_03_0238:

DLI Failed to Connect to GaussDB(DWS) Through an Enhanced Datasource Connection
===============================================================================

Symptom
-------

The outbound rule had been configured for the security group of the queue associated with the enhanced datasource connection. The datasource authentication used a password. The connection failed and **DLI.0999: PSQLException: The connection attempt failed** is reported.

Cause Analysis
--------------

Possible causes are as follows:

-  The security group configuration is incorrect.
-  The subnet configuration is incorrect.

Procedure
---------

#. Check whether the security group is accessible.

   -  Inbound rule: Check whether the inbound CIDR block and port in the security group have been enabled. If not, create the CIDR block and port you need.
   -  Outbound rule: Check whether the CIDR block and port of the outbound rule are enabled. (It is recommended that all CIDR blocks be enabled.)

   Both the inbound and outbound rules of the security group are configured for the subnets of the DLI queue. Set the source IP address in the inbound direction to 0.0.0.0/0 and port 8000, indicating that any IP address can access port 8000.

#. If the fault persists, check the subnet configuration. Check the network ACL associated with the GaussDB(DWS) subnet. A network ACL is an optional layer of security for your subnets. You can associate one or more subnets with a network ACL to control traffic in and out of the subnets. After the association, the network ACL denies all traffic to and from the subnet by default until you add rules to allow traffic. The check result showed that the ACL associated with the subnet where GaussDB(DWS) resides is empty.

   A network ACL is associated and no inbound or outbound rules are configured. As a result, the IP address cannot be accessed.

#. Perform the connectivity test. After the subnet inbound and outbound rules are configured, the datasource connection passes the connectivity test.
