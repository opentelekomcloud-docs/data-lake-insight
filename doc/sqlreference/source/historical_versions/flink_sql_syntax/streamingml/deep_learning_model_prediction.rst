:original_name: dli_08_0088.html

.. _dli_08_0088:

Deep Learning Model Prediction
==============================

Deep learning has a wide range of applications in many industries, such as image classification, image recognition, and speech recognition. DLI provides several functions to load deep learning models for prediction.

Currently, models DeepLearning4j and Keras are supported. In Keras, TensorFlow, CNTK, or Theano can serve as the backend engine. With importing of the neural network model from Keras, models of mainstream learning frameworks such as Theano, TensorFlow, Caffe, and CNTK can be imported.

Syntax
------

::

   -- Image classification: returns the predicted category IDs used for image classification.
   DL_IMAGE_MAX_PREDICTION_INDEX(field_name, model_path, is_dl4j_model)
   DL_IMAGE_MAX_PREDICTION_INDEX(field_name, keras_model_config_path, keras_weights_path) -- Suitable for the Keras model

   --Text classification: returns the predicted category IDs used for text classification.
   DL_TEXT_MAX_PREDICTION_INDEX(field_name, model_path, is_dl4j_model) -- Use the default word2vec model.
   DL_TEXT_MAX_PREDICTION_INDEX(field_name, word2vec_path, model_path, is_dl4j_model)

.. note::

   Models and configuration files must be stored on OBS. The path format is obs://**your_ak**:**your_sk**\ @obs.\ **your_obs_region**.xxx.com:443/**your_model_path**.

Parameter Description
---------------------

.. table:: **Table 1** Parameter description

   +-------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter               | Mandatory             | Description                                                                                                                         |
   +=========================+=======================+=====================================================================================================================================+
   | field_name              | Yes                   | Name of the field, data in which is used for prediction, in the data stream.                                                        |
   |                         |                       |                                                                                                                                     |
   |                         |                       | In image classification, this parameter needs to declare ARRAY[TINYINT].                                                            |
   |                         |                       |                                                                                                                                     |
   |                         |                       | In image classification, this parameter needs to declare String.                                                                    |
   +-------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | model_path              | Yes                   | Complete save path of the model on OBS, including the model structure and model weight.                                             |
   +-------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | is_dl4j_model           | Yes                   | Whether the model is a Deeplearning4j model                                                                                         |
   |                         |                       |                                                                                                                                     |
   |                         |                       | Value **true** indicates that the model is a Deeplearning4j model, while value **false** indicates that the model is a Keras model. |
   +-------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | keras_model_config_path | Yes                   | Complete save path of the model structure on OBS. In Keras, you can obtain the model structure by using **model.to_json()**.        |
   +-------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | keras_weights_path      | Yes                   | Complete save path of the model weight on OBS. In Keras, you can obtain the model weight by using **model.save_weights(filepath)**. |
   +-------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | word2vec_path           | Yes                   | Complete save path of the word2vec model on OBS.                                                                                    |
   +-------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

For prediction in image classification, use the Mnist dataset as the input and load the pre-trained Deeplearning4j model or Keras model to predict the digit representing each image in real time.

::

   CREATE SOURCE STREAM Mnist(
       image Array[TINYINT]
   )
   SELECT DL_IMAGE_MAX_PREDICTION_INDEX(image, 'your_dl4j_model_path', false) FROM Mnist
   SELECT DL_IMAGE_MAX_PREDICTION_INDEX(image, 'your_keras_model_path', true) FROM Mnist
   SELECT DL_IMAGE_MAX_PREDICTION_INDEX(image, 'your_keras_model_config_path', 'keras_weights_path') FROM Mnist

For prediction in text classification, use data of a group of news titles as the input and load the pre-trained Deeplearning4j model or Keras model to predict the category of each news title in real time, such as economy, sports, and entertainment.

::

   CREATE SOURCE STREAM News(
       title String
   )
   SELECT DL_TEXT_MAX_PREDICTION_INDEX(title, 'your_dl4j_word2vec_model_path','your_dl4j_model_path', false) FROM News
   SELECT DL_TEXT_MAX_PREDICTION_INDEX(title, 'your_keras_word2vec_model_path','your_keras_model_path', true) FROM News
   SELECT DL_TEXT_MAX_PREDICTION_INDEX(title, 'your_dl4j_model_path', false) FROM New
   SELECT DL_TEXT_MAX_PREDICTION_INDEX(title, 'your_keras_model_path', true) FROM New
