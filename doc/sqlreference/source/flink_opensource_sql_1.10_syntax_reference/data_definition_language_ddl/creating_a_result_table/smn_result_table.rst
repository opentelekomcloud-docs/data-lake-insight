:original_name: dli_08_0314.html

.. _dli_08_0314:

SMN Result Table
================

Function
--------

DLI exports Flink job output data to SMN.

SMN provides reliable and flexible large-scale message notification services to DLI. It significantly simplifies system coupling and pushes messages to subscription endpoints based on requirements. SMN can be connected to other cloud services or integrated with any application that uses or generates message notifications to push messages over multiple protocols.

Syntax
------

.. code-block::

   create table smnSink (
     attr_name attr_type
     (',' attr_name attr_type)*
     (','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
   )
   with (
     'connector.type' = 'smn',
     'connector.region' = '',
     'connector.topic-urn' = '',
     'connector.message-subject' = '',
     'connector.message-column' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +---------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                 | Mandatory             | Description                                                                                                                                                                       |
   +===========================+=======================+===================================================================================================================================================================================+
   | connector.type            | Yes                   | Sink data type. Set this parameter to **smn**, which means that data is stored to SMN.                                                                                            |
   +---------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.region          | Yes                   | Region where SMN belongs                                                                                                                                                          |
   +---------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.topic-urn       | No                    | URN of an SMN topic, which is used for the static topic URN configuration. The SMN topic serves as the destination for short message notification and needs to be created in SMN. |
   |                           |                       |                                                                                                                                                                                   |
   |                           |                       | Either of **topic_urn** and **urn_column** must be configured. If both of them are configured, the **topic_urn** setting takes precedence.                                        |
   +---------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.urn-column      | No                    | Field name of the topic URN content, which is used for the dynamic topic URN configuration.                                                                                       |
   |                           |                       |                                                                                                                                                                                   |
   |                           |                       | One of **topic_urn** and **urn_column** must be configured. If both of them are configured, the **topic_urn** setting takes precedence.                                           |
   +---------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.message-subject | Yes                   | Message subject sent by SMN. This parameter can be customized.                                                                                                                    |
   +---------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.message-column  | Yes                   | Column name in the current table. Data in this column is the message content and is customized. Currently, only text messages are supported.                                      |
   +---------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

None

Example
-------

Write the data to the target of SMN topic. The topic of the message sent by SMN is **test**, and the message content is the data in the **attr1** column.

.. code-block::

   create table smnSink (
     attr1 STRING,
     attr2 STRING
   )
   with (
     'connector.type' = 'smn',
     'connector.region' = '',
     'connector.topic-urn' = 'xxxxxx',
     'connector.message-subject' = 'test',
     'connector.message-column' = 'attr1'
   );
