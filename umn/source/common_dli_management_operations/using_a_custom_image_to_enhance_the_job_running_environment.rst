:original_name: dli_01_0494.html

.. _dli_01_0494:

Using a Custom Image to Enhance the Job Running Environment
===========================================================

Scenario
--------

To enhance the functions and performance of Spark and Flink jobs, you can create custom images by downloading the base images provided by DLI and adding dependencies (files, JAR files, or software) and private capabilities required for job execution. This changes the container runtime environment for the jobs.

For example, you can add a Python package or C library related to machine learning to a custom image to help you extend functions.

.. note::

   To use the custom image function, you need to have basic knowledge of `Docker <https://www.docker.com/>`__.

Notes and Constraints
---------------------

-  The base images provided by DLI must be used to create custom images.
-  You cannot modify the DLI components and directories in the base images.
-  Only Spark Jar and Flink Jar jobs are supported.

Use Process
-----------


.. figure:: /_static/images/en-us_image_0000001940346350.png
   :alt: **Figure 1** Process of using a custom image

   **Figure 1** Process of using a custom image

#. Obtain DLI base images.
#. Use Dockerfile to pack dependencies (files, JAR files, or software) required for job execution into the base image to create a custom image.
#. Publish the custom image to SoftWare Repository for Container (SWR).
#. On the DLI job editing page, select the created image and run the job.
#. Check the job execution status.

.. _dli_01_0494__section15325113611492:

Obtaining DLI Base Images
-------------------------

Contact the administrator to obtain DLI base images.

Select the base image of the same type as the architecture of the queue.

For the CPU architecture type of a queue, see :ref:`Viewing Basic Information About a Queue <dli_01_0663>`.

Creating a Custom Image
-----------------------

The following describes how to package TensorFlow into an image to generate a custom image with TensorFlow installed. Then, you can use the image to run jobs in DLI.

#. .. _dli_01_0494__li196252035104114:

   Prepare the container environment.

#. Log in to the prepared container environment as user **root** and run a command to obtain the base image.

   In this example, the Spark base image is used and downloaded to the container image environment in :ref:`1 <dli_01_0494__li196252035104114>` by running the following command:

   **docker pull** *Address for downloading the base image*

   For details about the address, see :ref:`Using a Custom Image to Enhance the Job Running Environment <dli_01_0494>`.

   For example, to download the Spark base image, run the following command:

   .. code-block::

      docker pull swr.xxx/dli-public/spark_general-x86_64:3.3.1-2.3.7.1720240419835647952528832.202404250955

#. Access SWR.

   a. Log in to the SWR management console.
   b. In the navigation pane on the left, choose **Dashboard** and click **Generate Login Command** in the upper right corner. On the displayed page, click |image1| to copy the login command.
   c. Run the login command on the VM where the container engine is installed.

#. Create an organization. If an organization has been created, skip this step.

   a. Log in to the SWR management console.
   b. In the navigation pane on the left, choose **Organization Management**. On the displayed page, click **Create Organization** in the upper right corner.
   c. Enter the organization name and click **OK**.

#. Write a Dockerfile.

   **vi Dockerfile**

   Pack TensorFlow into the image as follows:

   .. code-block::

      ARG BASE_IMG=swr.xxx/dli-public/spark_general-x86_64:3.3.1-2.3.7.1720240419835647952528832.202404250955//Replace xxx with the URL of the base image.

      FROM ${BASE_IMG} as builder
      USER omm //Run this command as user omm.
      RUN set -ex && \
          mkdir -p /home/omm/.pip && \
          pip3 install tensorflow==2.4.0
      Copy the content to the base image.
      USER omm

   The following steps are included:

   a. Set the available repository address of pip.
   b. Use pip3 to install the TensorFlow algorithm package.
   c. Copy the content in the temporary image builder where the algorithm package is installed to the base image (this step is to reduce the image size) to generate the final custom image.

#. .. _dli_01_0494__li184555913158:

   Use Dockerfile to generate a custom image.

   Format of the image packaging command:

   .. code-block::

      docker build -t [Custom organization name]/[Custom image name]: [Image version] --build-arg BASE_IMG= [DLI base image path] -f Dockerfile .

   The DLI base image path is the image path in :ref:`Obtaining DLI Base Images <dli_01_0494__section15325113611492>`.

   The following is an example:

   .. code-block::

      docker build -t mydli/spark:2.4 --build-arg BASE_IMG=swr.xxx/dli-public/spark_general-x86_64:3.3.1-2.3.7.1720240419835647952528832.202404250955 -f Dockerfile .

#. .. _dli_01_0494__li14517172710562:

   Add a tag to the custom image.

   **docker tag** [Organization name]/[Image name]:[Image version][Image repository address]/[Organization name]/[Image name:version] in :ref:`6 <dli_01_0494__li184555913158>`

   The following is an example:

   .. code-block::

      docker tag mydli/spark:2.4 swr.xxx/testdli0617/spark:2.4.5.tensorflow

#. Upload the custom image.

   **docker push** [Image repository address]/[Organization name]/[Image name:Version]

   Set [Image repository address]/[Organization name]/[Image name:Version] the same as those in :ref:`7 <dli_01_0494__li14517172710562>`.

   The following is an example:

   .. code-block::

      docker push swr.xxx/testdli0617/spark:2.4.5.tensorflow

#. When submitting a Spark or Flink JAR job in DLI, select a custom image.

   -  Open the Spark job or Flink job editing page on the management console, select the uploaded and shared image from the custom image list, and run the job.

      If you select a non- shared image, the system displays a message indicating that the image is not authorized. You can use the image only after it is authorized.

   -  Specify the image parameter in job parameters on API to use a custom image to run a job.

.. |image1| image:: /_static/images/en-us_image_0000001336444477.png
