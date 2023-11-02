:original_name: dli_08_0273.html

.. _dli_08_0273:

Custom Source Stream
====================

Compile code to obtain data from the desired cloud ecosystem or open-source ecosystem as the input data of Flink jobs.

Syntax
------

::

   CREATE SOURCE STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     WITH (
       type = "user_defined",
       type_class_name = "",
       type_class_parameter = ""
     )
     (TIMESTAMP BY timeindicator (',' timeindicator)?);timeindicator:PROCTIME '.' PROCTIME| ID '.' ROWTIME

Keyword
-------

.. table:: **Table 1** Keyword description

   +----------------------+-----------+------------------------------------------------------------------------------------------------------------+
   | Parameter            | Mandatory | Description                                                                                                |
   +======================+===========+============================================================================================================+
   | type                 | Yes       | Data source type. The value **user_defined** indicates that the data source is a user-defined data source. |
   +----------------------+-----------+------------------------------------------------------------------------------------------------------------+
   | type_class_name      | Yes       | Name of the source class for obtaining source data. The value must contain the complete package path.      |
   +----------------------+-----------+------------------------------------------------------------------------------------------------------------+
   | type_class_parameter | Yes       | Input parameter of the user-defined source class. Only one parameter of the string type is supported.      |
   +----------------------+-----------+------------------------------------------------------------------------------------------------------------+

Precautions
-----------

The user-defined source class needs to inherit the **RichParallelSourceFunction** class and specify the data type as Row. For example, define MySource class: **public class MySource extends RichParallelSourceFunction<Row>{}**. It aims to implement the **open**, **run**, and **close** functions.

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

A data record is generated in each period. The data record contains only one field of the INT type. The initial value is 1 and the period is 60 seconds. The period is specified by an input parameter.

::

   CREATE SOURCE STREAM user_in_data (
       count INT
        )
     WITH (
       type = "user_defined",
       type_class_name = "mySourceSink.MySource",
       type_class_parameter = "60"
         )
         TIMESTAMP BY car_timestamp.rowtime;

.. note::

   To customize the implementation of the source class, you need to pack the class in a JAR package and upload the UDF function on the SQL editing page.
