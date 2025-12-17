:original_name: dli_01_0622.html

.. _dli_01_0622:

Checking Basic Information
==========================

After creating an elastic resource pool, you can check and manage it on the management console.

This section describes how to check basic information about an elastic resource pool on the management console, including the VPC CIDR block, IPv6 CIDR block, and creation time.

Procedure
---------

#. Log in to the DLI management console.

#. In the navigation pane on the left, choose **Resources** > **Resource Pool**.

#. On the displayed page, select the elastic resource pool you want to check.

   -  In the upper right corner of the list page, click |image1| to customize the columns to display and set the rules for displaying the table content and the **Operation** column.
   -  In the search box above the list, you can filter the required elastic resource pool by name or tag.

#. Click |image2| to expand the basic information card of the elastic resource pool and view detailed information about the pool.

   These details include the name of the pool, the user who created it, the date it was created, and the VPC CIDR block.

   For the definitions of actual CUs, used CUs, CU range, and specifications of the elastic resource pool, refer to :ref:`Actual CUs, Used CUs, CU Range, and Specifications of an Elastic Resource Pool <dli_01_0622__section3723248135610>`.

.. _dli_01_0622__section3723248135610:

Actual CUs, Used CUs, CU Range, and Specifications of an Elastic Resource Pool
------------------------------------------------------------------------------

-  **Actual CUs**: actual size of resources currently allocated to the elastic resource pool (in CUs).

   -  When there is no queue in the resource pool, the actual CUs are equal to the minimum CUs when the elastic resource pool is created.

   -  When there are queues in the resource pool, the formula for calculating actual CUs is:

      -  Actual CUs = max{(min[sum(maximum CUs of queues), maximum CUs of the elastic resource pool]), minimum CUs of the elastic resource pool}.
      -  The calculation result must be a multiple of 16 CUs. If it cannot be exactly divided by 16 CUs, round up to the nearest multiple.

   -  Scaling out or in an elastic resource pool means adjusting the actual CUs of the resource pool. Refer to :ref:`Scaling Out or In an Elastic Resource Pool <dli_01_0686>`.

   -  Example of actual CU allocation:

      In :ref:`Table 1 <dli_01_0622__dli_07_0003_dli_07_0003_table4844638152415>`, the calculation process for the actual allocation of CUs in an elastic resource pool is as follows:

      #. Calculate the sum of maximum CUs of the queues: sum(maximum CUs) = 32 + 56 = 88 CUs.

      #. Compare the sum of maximum CUs of the queues with the maximum CUs of the elastic resource pool and take the smaller value: min{88 CUs, 112 CUs} = 88 CUs.

      #. Compare the value with the minimum CUs of the elastic resource pool and take the larger value: max(88 CUs, 64 CUs) = 88 CUs.

      #. Check if 88 CUs is a multiple of 16 CUs. Since 88 is not divisible by 16, round up to 96 CUs.

         .. _dli_01_0622__dli_07_0003_dli_07_0003_table4844638152415:

         .. table:: **Table 1** Example of actual CU allocation of an elastic resource pool

            +---------------------------------------------------------------------------------------------------+-----------------------+-----------------------+
            | Scenario                                                                                          | Resource Type         | CU Range              |
            +===================================================================================================+=======================+=======================+
            | New elastic resource pool: 64-112 CUs                                                             | Elastic resource pool | 64-112 CUs            |
            |                                                                                                   |                       |                       |
            | Queues A and B are created within the elastic resource pool. The CU ranges of the two queues are: |                       |                       |
            |                                                                                                   |                       |                       |
            | -  CU range of queue A: 16-32 CUs                                                                 |                       |                       |
            | -  CU range of queue B: 16-56 CUs                                                                 |                       |                       |
            +---------------------------------------------------------------------------------------------------+-----------------------+-----------------------+
            |                                                                                                   | Queue A               | 16-32 CUs             |
            +---------------------------------------------------------------------------------------------------+-----------------------+-----------------------+
            |                                                                                                   | Queue B               | 16-56CUS              |
            +---------------------------------------------------------------------------------------------------+-----------------------+-----------------------+

-  **Used CUs**: CUs that have been used by jobs or tasks. These resources may be executing computing tasks.
-  **CU range**: CU settings are used to control the maximum and minimum CU ranges for elastic resource pools to avoid unlimited resource scaling.

   -  The total minimum CUs of all queues in an elastic resource pool must be no more than the minimum CUs of the pool.
   -  The maximum CUs of any queue in an elastic resource pool must be no more than the maximum CUs of the pool.
   -  An elastic resource pool should at least ensure that all queues in it can run with the minimum CUs and should try to ensure that all queues in it can run with the maximum CUs.
   -  When expanding the specifications of an elastic resource pool, the minimum value of the CU range is linked to the specifications of the elastic resource pool. After changing the specifications of the elastic resource pool, the minimum value of the CU range is modified to match the specifications.

-  **Specifications**: The minimum CUs selected during elastic resource pool purchase are elastic resource pool specifications.

.. |image1| image:: /_static/images/en-us_image_0000001934973209.png
.. |image2| image:: /_static/images/en-us_image_0000002129969110.png
