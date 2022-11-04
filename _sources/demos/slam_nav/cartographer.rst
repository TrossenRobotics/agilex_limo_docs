==========================
Mapping using Cartographer
==========================

.. contents::
    :local:

Overview
========

.. TODO(lsinterbotix): write a better overview here

Cartographer is a set of SLAM algorithms based on image optimization launched by Google. The main
goal of this algorithm is to achieve low computing resource consumption while simultaneously
mapping and localizing in real-time. The algorithm is divided into two parts. The first part is
called Local SLAM. This part establishes and maintains a series of submaps through a laser scan
frame, and the so-called submap is a series of Grid Maps. The second part of the algorithm, called
Global SLAM, is to perform closed-loop detection through Loop Closure to eliminate accumulated
errors: when a submap is built, no new laser scans will be inserted into the submap. The algorithm
will add the submap to the closed-loop detection.

You can find more information on the `cartographer`_ and `cartographer_ros`_ GitHub repositories.

.. _`cartographer`: https://github.com/cartographer-project/cartographer
.. _`cartographer_ros`: https://github.com/cartographer-project/cartographer_ros

Structure
=========

Usage
=====

.. note::

    Use :kbd:`Ctrl` + :kbd:`C` command to end all the processes before running the below commands.

.. note::

    Mapping works best when the LIMO drives slowly through its environment. If it moves around too
    quickly, mapping quality will be affected.

Mapping
-------

1.  Start the LIMO. Open a new terminal, and enter the command:

    .. code-block::

        $ roslaunch limo_bringup limo_start.launch pub_odom_tf:=false

2.  Launch the Cartographer processes. In a new terminal, enter the command:

    .. code-block::

        $ roslaunch limo_bringup limo_cartographer.launch

    After launching successfully, RViz will be opened and display something like the image below.

    .. image:: _images/cartographer_1.png
        :align: center
        :width: 80%

3.  Use the mobile app to explore and map out the environment.

Saving the Map
--------------

After building the map, it must be saved. Do the following to save the map to the specified
directory and file:

1.  Call the ``/finish_trajectory`` service to notify Cartographer that no further data should be
    accepted:

    .. code-block::

        $ rosservice call /finish_trajectory 0

2.  Serialize the map and save its current state by calling the ``/write_state`` service:

    .. code-block::

        $ rosservice call /write_state "{filename: '~/agilex_ws/src/limo_ros/limo_bringup/maps/mymap.pbstream'}"

3.  Convert pbstream to pgm and yaml using the ``cartographer_pbstream_to_ros_map`` script:

    .. code-block::

        $ rosrun cartographer_ros cartographer_pbstream_to_ros_map -map_filestem=~/agilex_ws/src/limo_ros/limo_bringup/maps/mymap.pbstream -pbstream_filename=~/agilex_ws/src/limo_ros/limo_bringup/maps/mymap.pbstream -resolution=0.05

    This command generates the corresponding pgm and yaml and saves them to the directory:

    .. code-block::

        ~/agilex_ws/src/limo_ros/limo_bringup/maps/mymap.pbstream

    .. note::

        The ``cartographer_pbstream_to_ros_map`` script takes the following arguments:

            *   **-map_filestem**: (string) Stem of the output files.
            *   **-pbstream_filename**: (string) Filename of a pbstream to draw a map from.
            *   **-resolution**: (double): Resolution of a grid cell in the drawn map.

.. note::

    During the process of mapping, some warnings may appear in the terminal. This is caused by
    excessive speed and delayed data processing. It can safely be ignored.

    .. image:: _images/cartographer_2.png
        :align: center

Configuration
=============

The Cartographer package can be configured using the launch file at
``~/agilex_ws/src/limo_ros/limo_bringup/launch/limo_cartographer.launch``

+----------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **Parameter**                    | **Default**     | **Description**                                                                                                                                                                                                                                                                                                                   |
+==================================+=================+===================================================================================================================================================================================================================================================================================================================================+
| map_frame                        | map             | The ID of the ROS coordinate system used to publish submaps, the parent coordinate system of the pose, usually "map".                                                                                                                                                                                                             |
+----------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| tracking_frame                   | base_footprint  | The ID of the ROS coordinate system tracked by the SLAM algorithm. If IMU is used, its coordinate system should be used, usually "imu_link".                                                                                                                                                                                      |
+----------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| published_frame                  | odom            | The ID of the ROS coordinate system used to publish the pose sub-coordinate system, like the "odom" coordinate system. If an "odom" coordinate system is provided by different parts of the system, in this case, the "odom" pose in the map_frame will be published. Otherwise, it may be appropriate to set it to "base_link".  |
+----------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| odom_frame                       | odom            | It is enabled when provide_odom_frame is true. The coordinate system is used to publish local SLAM results between published_frame and map_frame, usually "odom".                                                                                                                                                                 |
+----------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| provide_odom_frame               | true            | If enabled, local, non-closed-loop, and continuous poses will be published as odom_frame in map_frame.                                                                                                                                                                                                                            |
+----------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| use_odometry                     | false           | If enabled, subscribe to nav_msgs/Odometry messages on the "odom" topic. The mileage information will be provided, which is included in SLAM.                                                                                                                                                                                     |
+----------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| num_laser_scans                  | 1               | The number of laser scanning topics subscribed. Subscribe to sensor_msgs/LaserScan on the "scan" topic of one laser scanner or subscribe to the topics "scan_1", "scan_2", etc. on multiple laser scanners.                                                                                                                       |
+----------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| num_multi_echo_laser_scans       | 0               | The number of subscribed multi-echo laser scanning topics. Subscribe to sensor_msgs/MultiEchoLaserScan on the "echoes" topic of a laser scanner or subscribe to the topics "echoes_1", "echoes_2", etc. for multiple laser scanners.                                                                                              |
+----------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| num_subdivisions_per_laser_scan  | 1               | The number of point clouds that divide each received (multi-echo) laser scan. The subdivision scan can cancel the scan acquired by the scan when the scanner is moving. There is a corresponding trajectory builder option to accumulate subdivision scans into the point cloud that will be used for scan matching.              |
+----------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| num_point_clouds                 | 0               | The number of point cloud topics to be subscribed to. Subscribe to sensor_msgs/PointCloud2 on the "points2" topic of a range finder or subscribe topics "points2_1", "points2_2", etc. for multiple range finders.                                                                                                                |
+----------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| lookup_transform_timeout_sec     | 0.2             | The timeout seconds of looking up and transforming with tf2.                                                                                                                                                                                                                                                                      |
+----------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| submap_publish_period_sec        | 0.3             | The period (in seconds) for publishing submaps, eg.0.3 seconds.                                                                                                                                                                                                                                                                   |
+----------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| pose_publish_period_sec          | 5e-3            | The period (in seconds) for publishing poses, eg. 5e-3, with a frequency of 200 Hz.                                                                                                                                                                                                                                               |
+----------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| trajectory_publish_period_sec    | 30e-3           | The period for publishing trajectory tag in seconds, eg. 30e-3, lasting 30 milliseconds.                                                                                                                                                                                                                                          |
+----------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
