===================================
Mapping & Navigation using RTAB-Map
===================================

.. contents::
    :local:

Overview
========

The RTAB-Map package provides an appearance-based positioning and mapping solution independent of
time and scale. It's aimed at solving the problem of online closed-loop detection in large-scale
environments. The idea of the solution is to meet some real-time limitations. Closed-loop detection
uses only a limited number of positioning points, while being able to access the positioning points
of the entire map when needed.

You can find more information on the `rtabmap`_ and `rtabmap_ros`_ GitHub repositories.

.. _`rtabmap`: https://github.com/introlab/rtabmap
.. _`rtabmap_ros`: https://github.com/introlab/rtabmap_ros

Structure
=========

Usage
=====

Mapping
-------

.. note::

    Use :kbd:`Ctrl` + :kbd:`C` command to end all the processes before running the below commands.

.. note::

    Mapping works best when the LIMO drives slowly through its environment. If it moves around too
    quickly, mapping quality will be affected.

1.  Start the LIMO. Open a new terminal, and enter the command:

    .. code-block::

        $ roslaunch limo_bringup limo_start.launch pub_odom_tf:=false

2.  Start the camera. Open a new terminal and enter the command:

    .. code-block::

        $  roslaunch astra_camera dabai_u3.launch

3.  Launch RTAB-Map in its mapping mode. Enter the command:

    .. code-block::

        roslaunch limo_bringup limo_rtabmap_orbbec.launch

    After launching successfully, RViz will be opened and display something like the image below.

    .. image:: _images/rtabmap_1.png
        :align: center
        :width: 80%

3.  Use the mobile app to explore and map out the environment.

Saving the Map
--------------

After building the map, you can directly terminate the program, and the map will automatically be
saved under the ``~/.ros`` directory as ``rtabmap.db``. The ``~/.ros`` folder is hidden and can be
displayed by the :kbd:`Ctrl` + :kbd:`H` command when in the file explorer.

Localization
------------

.. note::

    Use :kbd:`Ctrl` + :kbd:`C` command to end all the processes before running the below commands.

1.  Start the LIMO. Open a new terminal, and enter the command:

    .. code-block::

        $ roslaunch limo_bringup limo_start.launch pub_odom_tf:=false

2.  Start the camera. Open a new terminal and enter the command:

    .. code-block::

        $ roslaunch astra_camera dabai_u3.launch

3.  Launch RTAB-Map's localization mode. Open a new terminal and enter the command:

    .. code-block::

        $ roslaunch limo_bringup limo_rtabmap_orbbec.launch localization:=true

4.  Launch move_base. Open a new terminal and enter the command:

    .. code-block::

        $ roslaunch limo_bringup limo_navigation_rtabmap.launch

    .. note::

        If in Ackermann steering mode, run the command:

        .. code-block::

            $ roslaunch limo_bringup limo_navigation_rtabmap_ackerman.launch

5.  Launch RViz to view the map and navigation tools. Open a new terminal and enter the command:

    .. code-block::

        $  roslaunch limo_bringup rtabmap_rviz.launch

    .. note::

        The robot may need to detect more visual features on initialization before it can localize
        itself. To do this, drive the robot around until it appears to be in the correct location
        in RViz. See `rtabmap_ros issue #220`_ for details and other potential workarounds.

.. _`rtabmap_ros issue #220`: https://github.com/introlab/rtabmap_ros/issues/220

6.  Use the **2D Nav Goal** button to set your navigation goal as shown below.

    .. image:: _images/rtabmap_2.png
        :align: center
        :width: 80%

    A green path will be displayed in the map indicating the planned path, and the robot will
    automatically navigate to the goal.

    .. image:: _images/rtabmap_3.png
        :align: center
        :width: 80%
