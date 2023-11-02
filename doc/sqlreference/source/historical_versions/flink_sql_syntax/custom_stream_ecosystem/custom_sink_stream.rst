:original_name: dli_08_0274.html

.. _dli_08_0274:

Custom Sink Stream
==================

Compile code to write the data processed by DLI to a specified cloud ecosystem or open-source ecosystem.

Syntax
------

.. code-block::

   CREATE SINK STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     WITH (
       type = "user_defined",
       type_class_name = "",
       type_class_parameter = ""
     );

Keyword
-------

.. table:: **Table 1** Keyword description

   +----------------------+-----------+------------------------------------------------------------------------------------------------------------+
   | Parameter            | Mandatory | Description                                                                                                |
   +======================+===========+============================================================================================================+
   | type                 | Yes       | Data source type. The value **user_defined** indicates that the data source is a user-defined data source. |
   +----------------------+-----------+------------------------------------------------------------------------------------------------------------+
   | type_class_name      | Yes       | Name of the sink class for obtaining source data. The value must contain the complete package path.        |
   +----------------------+-----------+------------------------------------------------------------------------------------------------------------+
   | type_class_parameter | Yes       | Input parameter of the user-defined sink class. Only one parameter of the string type is supported.        |
   +----------------------+-----------+------------------------------------------------------------------------------------------------------------+

Precautions
-----------

The user-defined sink class needs to inherit the **RichSinkFunction** class and specify the data type as Row. For example, define MySink class: **public class MySink extends RichSinkFunction<Row>{}**. It aims to implement the **open**, **invoke**, and **close** functions.

Dependency pom:

.. code-block::

   <dependency>
       <groupId>org.apache.flink</groupId>
       <artifactId>flink-streaming-java_2.11</artifactId>
       <version>${flink.version}</version>
       <scope>provided</scope>
   </dependency>
   <dependency>
       <groupId>org.apache.flink</groupId>
       <artifactId>flink-core</artifactId>
       <version>${flink.version}</version>
       <scope>provided</scope>
   </dependency>

Example
-------

Writing data encoded in CSV format to a DIS stream is used as an example.

::

   CREATE SINK STREAM user_out_data (
       count INT
   )
     WITH (
       type = "user_defined",
       type_class_name = "mySourceSink.MySink",
       type_class_parameter = ""
         );

.. note::

   To customize the implementation of the sink class, you need to pack the class in a JAR package and upload the UDF function on the SQL editing page.
