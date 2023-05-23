:original_name: dli_01_0421.html

.. _dli_01_0421:

Creating a Message Notification Topic
=====================================

Scenario
--------

Once you have created a message notification topic, you can **Add subscription** of the topic on the **Topic Management** page of the Simple Message Notification service. You can select different ways (such as text messages or emails) to subscribe. After the subscription succeeds, any job failure will automatically be sent to your subscription endpoints.

Procedure
---------

#. On the **Queue Management** page, click **Create SMN Topic** on the upper left side. The **Create SMN Topic** dialog box is displayed.
#. Select a queue and click **OK**.

   .. note::

      -  You can select a single queue or all queues.
      -  If you create a topic for a queue and another topic for all queues, the SMN of all queues does not include the message of the single queue.
      -  After a notification topic is created, you will receive a notification only when a session or a batch job fails to be created.

#. Click **Topic Management** in the prompt for a successfully created topic to go to the **Topic Management** page of SMN.
#. In the **Operation** column of the topic, click **Add Subscription**. Select **Protocol** to determine the subscription mode.
#. After you click the link in the email, you will receive a message indicating that the subscription is successful.
#. Go to the **Subscriptions** page of SMN, and check that subscription status is **Confirmed**.
