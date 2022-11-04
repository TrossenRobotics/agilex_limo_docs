=======================
Lifting Barrier Control
=======================

.. contents::
    :local:

Overview
========

LIMO can determine the distance to the lifting barrier by detecting the AR tag on it. When the
distance to the tag is less than 0.3m, LIMO will send a message to the topic ``/chatter_updown`` to
raise or lower the barrier.

This demo uses the `ar_track_alvar`_ ROS package to determine the transform (translation, rotation)
between the robot's camera and the Alvar AR tag with ID 0, seen below. This tag should be places on
the lifting barrier before you begin this demo.

.. image:: _images/alvar0.png
    :align: center

.. _`ar_track_alvar`: http://wiki.ros.org/ar_track_alvar

Structure
=========

.. TODO(lsinterbotix): Add structure diagram

Usage
=====

.. note::

    Use :kbd:`Ctrl` + :kbd:`C` command to end all the processes before running the below commands.

Start the ORBBEC® Dabai camera:

.. code-block::

    $ roslaunch astra_camera dabai_u3.launch

Start AR tag detection:

.. code-block::

    $ roslaunch detect_ros agx_ar_pose.launch

When ``pub num :1`` is shown in the terminal, the lifting barrier will be raised. The LIMO will
then have three seconds to pass through the lifting barrier before it lowers again.
