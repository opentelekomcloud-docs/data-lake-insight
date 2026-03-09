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

   These details include the name of the pool, the user who created it, the date it was created, whether IPv6 is enabled, and the VPC CIDR block. If IPv6 is enabled, the subnet's IPv6 CIDR block will also be displayed.

   For details about the definitions of actual CUs, used CUs, CU range, and yearly/monthly CUs (specifications) of the elastic resource pool, see :ref:`Actual CUs, Used CUs, CU Range, and Yearly/Monthly CUs (Specifications) of an Elastic Resource Pool <dli_01_0622__section3723248135610>`.

.. _dli_01_0622__section3723248135610:

Actual CUs, Used CUs, CU Range, and Yearly/Monthly CUs (Specifications) of an Elastic Resource Pool
---------------------------------------------------------------------------------------------------

-  **Actual CUs**: The current allocated resource size of the elastic resource pool, measured in CUs.

   -  When no queues exist in the resource pool: The actual CUs equal the minimum CUs set during its creation.

   -  When there are queues in the resource pool, the formula to calculate actual CUs is:

      -  Actual CUs = max{(min[sum(maximum CUs of queues), maximum CUs of the elastic resource pool]), minimum CUs of the elastic resource pool}.
      -  The result must be a multiple of 16 CUs. If not divisible by 16, round up to the nearest multiple.

   -  Scaling out or in an elastic resource pool means adjusting its actual CUs. See :ref:`Scaling Out or In an Elastic Resource Pool <dli_01_0686>`.

   -  Example of actual CU allocation:

      Consider :ref:`Table 1 <dli_01_0622__dli_07_0003_dli_07_0003_table4844638152415>` below, which illustrates the process of calculating actual CUs for an elastic resource pool:

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

-  **Used CUs**: The portion of CUs currently occupied by jobs or tasks, which may be actively performing computations.
-  **CU range**: CU settings are used to control the maximum and minimum CU ranges for elastic resource pools to avoid unlimited resource scaling.

   -  The sum of all queues' minimum CUs in an elastic resource pool must not exceed the pool's minCU.
   -  Any single queue's maxCU cannot exceed the pool's maxCU.
   -  The resource pool ensures it meets the minCU requirements across all queues while striving to accommodate their maxCU demands.
   -  When expanding the specifications of an elastic resource pool, the minimum value of the CU range is linked to the yearly/monthly CUs (specifications) of the elastic resource pool. After changing the specifications of the elastic resource pool, the minimum value of the CU range is modified to match the yearly/monthly CUs (specifications).

-  **Yearly/monthly CUs (specifications)**: The minimum value of the CU range selected when purchasing an elastic resource pool is the elastic resource pool specifications.

.. |image1| image:: /_static/images/en-us_image_0000001934973209.png
.. |image2| image:: /_static/images/en-us_image_0000002129969110.png
