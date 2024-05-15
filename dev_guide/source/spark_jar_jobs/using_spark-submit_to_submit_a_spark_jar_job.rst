:original_name: dli_09_0122.html

.. _dli_09_0122:

Using Spark-submit to Submit a Spark Jar Job
============================================

Introduction to DLI Spark-submit
--------------------------------

DLI Spark-submit is a command line tool used to submit Spark jobs to the DLI server. This tool provides command lines compatible with open-source Spark.

Preparations
------------

#. Getting authorized.

   DLI uses the Identity and Access Management (IAM) to implement fine-grained permissions for your enterprise-level tenants. IAM provides identity authentication, permissions management, and access control, helping you securely access your resources.

   With IAM, you can use your account to create IAM users for your employees, and assign permissions to the users to control their access to specific resource types.

   Currently, roles (coarse-grained authorization) and policies (fine-grained authorization) are supported.

#. Create a queue. Choose **Resources** > **Queue Management**. On the page displayed, click **Buy Queue** in the upper right corner. On the **Buy Queue** page displayed, select **For general purpose** for **Type**, that is, the compute resources of the Spark job.

   .. note::

      If the user who creates the queue is not an administrator, the queue can be used only after being authorized by the administrator. For details about how to assign permissions, see .

Downloading the DLI Client Tool
-------------------------------

You can download the DLI client tool from the DLI management console.

#. Log in to the DLI management console.
#. Obtain the SDK download address from the administrator.
#. On the **DLI SDK DOWNLOAD** page, click **dli-clientkit-<**\ *version*\ **>** to download the DLI client tool.

   .. note::

      The Beeline client is named **dli-clientkit-<**\ *version*\ **>-bin.tar.gz**, which can be used in Linux and depends on JDK 1.8 or later.

Configuring DLI Spark-submit
----------------------------

Ensure that you have installed JDK 1.8 or later and configured environment variables on the computer where spark-submit is installed. You are advised to use spark-submit on the computer running Linux.

#. Download and decompress **dli-clientkit-<**\ *version*\ **>-bin.tar.gz**. In this step, set *version* to the actual version.

#. Go to the directory where **dli-clientkit-<version>-bin.tar.gz** is decompressed. In the directory, there are three subdirectories **bin**, **conf**, and **lib**, which respectively store the execution scripts, configuration files, and dependency packages related to **Spark-submit**.

#. Go to the **conf** directory and modify the configuration items in the **client.properties** file. For details about the configuration items, see :ref:`Table 1 <dli_09_0122__en-us_topic_0192429929_table571552873114>`.

   .. _dli_09_0122__en-us_topic_0192429929_table571552873114:

   .. table:: **Table 1** DLI client parameters

      +-----------------+-----------------+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Item            | Mandatory       | Default Value              | Description                                                                                                                                                                                                                |
      +=================+=================+============================+============================================================================================================================================================================================================================+
      | dliEndPont      | No              | ``-``                      | Domain name of DLI                                                                                                                                                                                                         |
      |                 |                 |                            |                                                                                                                                                                                                                            |
      |                 |                 |                            | If you lef this parameter empty, the program determines the domain name based on **region**.                                                                                                                               |
      +-----------------+-----------------+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | obsEndPoint     | Yes             | ``-``                      | OBS service domain name.                                                                                                                                                                                                   |
      |                 |                 |                            |                                                                                                                                                                                                                            |
      |                 |                 |                            | Obtain the OBS domain name from the administrator.                                                                                                                                                                         |
      +-----------------+-----------------+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | bucketName      | Yes             | ``-``                      | Name of a bucket on OBS. This bucket is used to store JAR files, Python program files, and configuration files used in Spark programs.                                                                                     |
      +-----------------+-----------------+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | obsPath         | Yes             | dli-spark-submit-resources | Directory for storing JAR files, Python program files, and configuration files on OBS. The directory is in the bucket specified by **Bucket Name**. If the directory does not exist, the program automatically creates it. |
      +-----------------+-----------------+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | localFilePath   | Yes             | ``-``                      | The local directory for storing JAR files, Python program files, and configuration files used in Spark programs.                                                                                                           |
      |                 |                 |                            |                                                                                                                                                                                                                            |
      |                 |                 |                            | The program automatically uploads the files on which Spark depends to the OBS path and loads them to the resource package on the DLI server.                                                                               |
      +-----------------+-----------------+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | ak              | Yes             | ``-``                      | User's Access Key (AK)                                                                                                                                                                                                     |
      +-----------------+-----------------+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | sk              | Yes             | ``-``                      | User's Secret Key (SK)                                                                                                                                                                                                     |
      +-----------------+-----------------+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | projectId       | Yes             | ``-``                      | Project ID used by a user to access DLI.                                                                                                                                                                                   |
      +-----------------+-----------------+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | region          | Yes             | ``-``                      | Region of interconnected DLI.                                                                                                                                                                                              |
      +-----------------+-----------------+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   Modify the configuration items in the **spark-defaults.conf** file based on the Spark application requirements. The configuration items are compatible with the open-source Spark configuration items. For details, see the open-source Spark configuration item description.

Using Spark-submit to Submit a Spark Job
----------------------------------------

#. Go to the **bin** directory of the tool file, run the **spark-submit** command, and carry related parameters.

   The command format is as follows:

   .. code-block::

      spark-submit [options] <app jar | python file> [app arguments]

   .. table:: **Table 2** DLI Spark-submit parameters

      +----------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                  | Value                 | Description                                                                                                                                                                                                                                                  |
      +============================+=======================+==============================================================================================================================================================================================================================================================+
      | --class                    | <CLASS_NAME>          | Name of the main class of the submitted Java or Scala application.                                                                                                                                                                                           |
      +----------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | --conf                     | <PROP=VALUE>          | Spark program parameters can be configured in the **spark-defaults.conf** file in the **conf** directory. If both the command and the configuration file are configured, the parameter value specified in the command is preferentially used.                |
      |                            |                       |                                                                                                                                                                                                                                                              |
      |                            |                       | .. note::                                                                                                                                                                                                                                                    |
      |                            |                       |                                                                                                                                                                                                                                                              |
      |                            |                       |    If there are multiple **conf** files, the format is **--conf key1=value1 --conf key2=value2**.                                                                                                                                                            |
      +----------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | --jars                     | <JARS>                | Name of the JAR file on which the Spark application depends. Use commas (,) to separate multiple names. The JAR file must be stored in the local path specified by **localFilePath** in the **client.properties** file in advance.                           |
      +----------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | --name                     | <NAME>                | Name of a Spark application.                                                                                                                                                                                                                                 |
      +----------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | --queue                    | <QUEUE_NAME>          | Name of the Spark queue on the DLI server. Jobs are submitted to the queue for execution.                                                                                                                                                                    |
      +----------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | --py-files                 | <PY_FILES>            | Name of the Python program file on which the Spark application depends. Use commas (,) to separate multiple file names. The Python program file must be saved in the local path specified by **localFilePath** in the **client.properties** file in advance. |
      +----------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | -s,--skip-upload-resources | <all \| app \| deps>  | Specifies whether to skip. Upload the JAR file, Python program file, and configuration file to OBS and load them to the resource list on the DLI server. If related resource files have been loaded to the DLI resource list, skip this step.                |
      |                            |                       |                                                                                                                                                                                                                                                              |
      |                            |                       | If this parameter is not specified, all resource files in the command are uploaded and loaded to DLI by default.                                                                                                                                             |
      |                            |                       |                                                                                                                                                                                                                                                              |
      |                            |                       | -  **all**: Skips the upload and loading all resource files.                                                                                                                                                                                                 |
      |                            |                       | -  **app**: Skips the upload and loading of Spark application files.                                                                                                                                                                                         |
      |                            |                       | -  **deps**: skips the upload and loading of all dependent files.                                                                                                                                                                                            |
      +----------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | -h,--help                  | ``-``                 | Displays command help information.                                                                                                                                                                                                                           |
      +----------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   Command example:

   .. code-block::

      ./spark-submit --name <name> --queue <queue_name> --class org.apache.spark.examples.SparkPi spark-examples_2.11-2.1.0.luxor.jar 10
      ./spark-submit --name <name> --queue <queue_name> word_count.py

   .. note::

      To use the DLI queue rather than the existing Spark environment, use **./spark-submit** instead of **spark-submit**.
