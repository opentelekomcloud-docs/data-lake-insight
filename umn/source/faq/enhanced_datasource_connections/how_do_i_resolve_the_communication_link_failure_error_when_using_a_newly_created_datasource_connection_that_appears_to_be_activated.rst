:original_name: dli_03_0047.html

.. _dli_03_0047:

How Do I Resolve the "communication link failure" Error When Using a Newly Created Datasource Connection That Appears to Be Activated?
======================================================================================================================================

Possible Causes
---------------

The network connectivity is abnormal. Check whether the security group is correctly selected and whether the VPC is correctly configured.

Solution
--------

Example: When you create an RDS datasource connection, the system displays the error message **Communication link failure**.

#. .. _dli_03_0047__li798113982620:

   Delete the original datasource connection and create a new one. When you create a connection, ensure that the selected **Security Group**, **VPC**, **Subnet**, and **Destination Address** are the same as those in RDS.

   .. note::

      Select a correct **Service Type**. In this example, select **RDS**.

#. Check the configurations of VPC.

   If the error message is still displayed after you create a datasource connection according to :ref:`Step 1 <dli_03_0047__li798113982620>`, check the VPC configuration.
