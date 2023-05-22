:original_name: dli_03_0047.html

.. _dli_03_0047:

Why Is the Error Message "communication link failure" Displayed When I Use a Newly Activated Datasource Connection?
===================================================================================================================

-  Possible Causes

   The network connectivity is abnormal. Check whether the security group is correctly selected and whether the VPC is correctly configured.

-  Solution

   Example: When you create an RDS datasource connection, the system displays the error message **Communication link failure**.

   #. .. _dli_03_0047__en-us_topic_0224125007_li798113982620:

      Delete the original datasource connection and create a new one. When you create a new connection, ensure that the selected **Security Group**, **VPC**, **Subnet**, and **Destination Address** are the same as those in RDS.

      .. note::

         Select a correct **Service Type**. In this example, select **RDS**.

   #. Check the configurations of VPC.

      If the error message is still displayed after you create a new datasource connection according to :ref:`Step 1 <dli_03_0047__en-us_topic_0224125007_li798113982620>`, check the VPC configuration.
