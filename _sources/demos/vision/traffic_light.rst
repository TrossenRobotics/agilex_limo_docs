============================
Traffic Light Identification
============================

.. contents::
    :local:

Overview
========

After the traffic lights are detected through darknet_ros, the traffic lights must be identified
and positioned in the three-dimensional space to generate the relative position of the object to
the camera. This method can only realize the identification and positioning of the traffic lights,
and cannot obtain the status of the traffic lights. Depth camera is needed, and its recognition
distance depends on the depth camera's range.

Structure
=========

Usage
=====

.. note::

    Use :kbd:`Ctrl` + :kbd:`C` command to end all the processes before running the below commands.

1.  Start the camera. Open a new terminal and enter the command:

    .. code-block::

        $ roslaunch astra_camera dabai_u3.launch

2.  Launch yolo_v3. Open a new terminal and enter the command:

    .. code-block:: console

        $ roslaunch darknet_ros  yolo_v3_tiny.launch

3.  Launch the traffic light recognition function. Open a new terminal and enter the command:

    .. code-block:: console

        $ roslaunch vision traffic_light_located.launch

.. TODO(lsinterbotix): take own screenshots

.. image:: _images/traffic_light.png
    :align: center
