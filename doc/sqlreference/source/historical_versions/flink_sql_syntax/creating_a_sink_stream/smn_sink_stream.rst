:original_name: dli_08_0251.html

.. _dli_08_0251:

SMN Sink Stream
===============

Function
--------

DLI exports Flink job output data to SMN.

SMN provides reliable and flexible large-scale message notification services to DLI. It significantly simplifies system coupling and pushes messages to subscription endpoints based on requirements. SMN can be connected to other cloud services or integrated with any application that uses or generates message notifications to push messages over multiple protocols.

For more information about SMN, see the .

Syntax
------

::

   CREATE SINK STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     WITH(
       type = "smn",
       region = "",
       topic_urn = "",
       urn_column = "",
       message_subject = "",
       message_column = ""
     )

Keywords
--------

.. table:: **Table 1** Keywords

   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                                       |
   +=======================+=======================+===================================================================================================================================================================================+
   | type                  | Yes                   | Output channel type. **smn** indicates that data is exported to SMN.                                                                                                              |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | region                | Yes                   | Region to which SMN belongs.                                                                                                                                                      |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | topic_urn             | No                    | URN of an SMN topic, which is used for the static topic URN configuration. The SMN topic serves as the destination for short message notification and needs to be created in SMN. |
   |                       |                       |                                                                                                                                                                                   |
   |                       |                       | One of **topic_urn** and **urn_column** must be configured. If both of them are configured, the **topic_urn** setting takes precedence.                                           |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | urn_column            | No                    | Field name of the topic URN content, which is used for the dynamic topic URN configuration.                                                                                       |
   |                       |                       |                                                                                                                                                                                   |
   |                       |                       | One of **topic_urn** and **urn_column** must be configured. If both of them are configured, the **topic_urn** setting takes precedence.                                           |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | message_subject       | Yes                   | Message subject sent to SMN. This parameter can be user-defined.                                                                                                                  |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | message_column        | Yes                   | Field name in the sink stream. Contents of the field name serve as the message contents, which are user-defined. Currently, only text messages (default) are supported.           |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

None

Example
-------

Data of stream **over_speed_warning** is exported to SMN.

::

   //Static topic configuration
   CREATE SINK STREAM over_speed_warning (
     over_speed_message STRING /* over speed message */
   )
     WITH (
       type = "smn",
       region = "xxx",
       topic_Urn = "xxx",
       message_subject = "message title",
       message_column = "over_speed_message"
     );

::

   //Dynamic topic configuration
   CREATE SINK STREAM over_speed_warning2 (
       over_speed_message STRING, /* over speed message */
       over_speed_urn STRING
   )
     WITH (
       type = "smn",
       region = "xxx",
       urn_column = "over_speed_urn",
       message_subject = "message title",
       message_column = "over_speed_message"
     );
