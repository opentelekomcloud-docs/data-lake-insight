:original_name: dli_01_0532.html

.. _dli_01_0532:

Viewing Scaling History
=======================

Scenario
--------

If you added a queue to or deleted one from an elastic resource pool, or you scaled an added queue, the CU quantity of the elastic resource pool may be changed. You can view historical CU changes of an elastic resource pool on the console.

.. caution::

   When scaling in an elastic resource pool, Spark and SQL jobs may automatically retry. However, if the number of retries exceeds the limit, the jobs will fail and need to be manually executed again.

Prerequisites
-------------

Currently, you can only view the historical records generated within the last 30 days on the console.

Viewing Scaling History of an Elastic Resource Pool
---------------------------------------------------

#. In the navigation pane on the left, choose **Resources** > **Resource Pool**.

#. Select the desired elastic resource pool and choose **More** > **Expansion History** in the **Operation** column.

#. On the displayed page, select a duration to view the CU usage.

   You can view the number of CUs before and after a scaling, and the target number of CUs.

   The historical records can be displayed in charts or tables. Click |image1| in the upper right corner to switch the display.

.. |image1| image:: /_static/images/en-us_image_0000001323141682.png
