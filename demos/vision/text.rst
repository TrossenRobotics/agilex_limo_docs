================
Text Recognition
================

.. contents::
    :local:

.. TODO(lsinterbotix): Add pictures, output to this demo

Overview
========

This text recognition demonstration does the following to get a string representation of text seen
through the onboard camera:

1.  Obtain an RBG image from the camera
2.  Perform `grayscale`_ and `binarization threshold`_ processing on the image
3.  Use the `pytesseract`_ text recognition library to recognize the English letters or numbers of
    the image
4.  Publish the recognized result to the ``/detect_word_reslut`` topic

.. _`pytesseract`: https://github.com/madmaze/pytesseract
.. _`grayscale`: https://en.wikipedia.org/wiki/Grayscale
.. _`binarization threshold`: https://en.wikipedia.org/wiki/Thresholding_(image_processing)

Structure
=========

.. TODO(lsinterbotix): Verify structure diagram
.. graphviz::
    :align: center

    digraph Text_Recognition_Pipeline {

        rankdir="LR";
        "camera" -> "/rgb_image" -> "detect_node" -> "/detect_word_reslut";

        "camera" [shape=ellipse];
        "/rgb_image" [shape=polygon,sides=4];
        "detect_node" [shape=ellipse]
        "/detect_word_reslut" [shape=polygon,sides=4];

    }

Usage
=====

.. note::

    Before running this demo, please make sure that all other programs have been terminated using
    :kbd:`Ctrl` + :kbd:`C`.

Start up ROS:

.. code-block::

    $ roscore

.. TODO(lsinterbotix): Add camera launch here

Start the text recognition node from the vision package by entering the command in the terminal:

.. code-block::

    $ rosrun vision detect_node.py

The output of the text recognition program is published to the topic ``/detect_word_reslut``. To
inspect the output, enter the following command:

.. code-block::

    $ rostopic echo /detect_word_reslut
