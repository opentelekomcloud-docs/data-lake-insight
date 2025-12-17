:original_name: dli_01_0421.html

.. _dli_01_0421:

Creating an SMN Topic
=====================

Scenario
--------

Once you have created an SMN topic, you can easily subscribe to it by going to the **Topic Management** > **Topics** page of the SMN console. You can choose to receive notifications via SMS or email. After the subscription is successful, if a job fails, the system automatically sends a message to the subscription endpoint you specified.

-  If a job fails within 1 minute of submission, a message notification is not triggered.
-  If a job fails after 1 minute of submission, the system automatically sends a message to the subscriber terminal you specified.

Procedure
---------

#. On the **Resources** > **Queue Management** page, click **Create SMN Topic** on the upper left side. The **Create SMN Topic** dialog box is displayed.
#. Select a queue and click **OK**.

   .. note::

      -  You can select a single queue or all queues.
      -  If you create a topic for a queue and another topic for all queues, the SMN of all queues does not include the message of the single queue.
      -  After a message notification topic is created, you will receive a message notification only when a Spark job created on the subscription queue fails.

#. Click **Topic Management** in to go to the **Topic Management** page of the SMN service.
#. In the **Operation** column of the topic, click **Add Subscription**. Select **Protocol** to determine the subscription mode.
#. After you click the link in the email, you will receive a message indicating that the subscription is successful.
#. Go to the **Subscriptions** page of SMN, and check that subscription status is **Confirmed**.
