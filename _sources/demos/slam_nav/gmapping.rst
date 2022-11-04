======================
Mapping using GMapping
======================

.. contents::
    :local:

Overview
========

GMapping is a commonly used open source SLAM algorithm based on the filtering SLAM framework.
GMapping effectively utilizes the wheel odometry information and does not require high frequency of
laser LiDAR. When mapping small environments, little processing power is required is low and the
accuracy is high.

You can find more information on the `openslam_gmapping`_ and `slam_gmapping`_ GitHub repositories.

.. _`openslam_gmapping`: https://github.com/OpenSLAM-org/openslam_gmapping
.. _`slam_gmapping`: https://github.com/ros-perception/slam_gmapping

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

2.  Launch the GMapping processes. In a new terminal, and enter the command:

    .. code-block::

        $ roslaunch limo_bringup limo_gmapping.launch

    After launching successfully, RViz will be opened and display something like the image below.

    .. image:: _images/gmapping.png
        :align: center
        :width: 80%

3.  Use the mobile app to explore and map out the environment.

Saving the Map
--------------

When finished mapping, do the following to save the map to the specified directory:

1.  Switch to the directory where you need to save the map. For example, if you want to save the map to
    ``~/agilex_ws/src/limo_ros/limo_bringup/maps/``, enter the command:

    .. code-block:: console

        cd ~/agilex_ws/src/limo_ros/limo_bringup/maps/

2.  After switching to ``~/agilex_ws/limo_bringup/maps``, enter the command:

    .. code-block:: console

        rosrun map_server map_saver -f map1

    This calls the map_saver script from the map_server package to save the map to a file in the
    current directory, in this case ``map1`` in ``~/agilex_ws/limo_bringup/maps``.

.. note::

    Use different names for different maps, else you risk overwriting existing map files.

Configuration
=============

The GMapping package can be configured using the launch file at
``~/agilex_ws/src/limo_ros/limo_bringup/launch/limo_gmapping.launch``

+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| **Parameter**              | **Type**  | **Default**  | **Description**                                                                                                                         |
+============================+===========+==============+=========================================================================================================================================+
| ~throttle_scans            | int       | 1            | The scan data threshold to be processed; the default is to process 1 scan data at a time (it can be set larger to skip some scan data)  |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~base_frame                | string    | base_link    | Robot base coordinate system                                                                                                            |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~map_frame                 | string    | map          | Map coordinate system                                                                                                                   |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~odom_frame                | string    | odom         | Odometer coordinate system                                                                                                              |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~map_update_interval       | float     | 5.0          | Map update frequency                                                                                                                    |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~maxUrange                 | float     | 80           | Detect the maximum available range, that is, the range that the beam can reach                                                          |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~sigma                     | float     | 0.05         | Standard deviation of endpoint matching                                                                                                 |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~kernelSize                | int       | 1            | Used to find the corresponding kernel size                                                                                              |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~lstep                     | float     | 0.05         | Translation optimization step                                                                                                           |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~astep                     | float     | 0.05         | Rotation optimization step                                                                                                              |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~iterations                | int       | 5            | Scan matching iterations                                                                                                                |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~lsigma                    | float     | 0.075        | Laser standard deviation for likelihood calculation                                                                                     |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~ogain                     | float     | 3.0          | Used for smooth resampling effect during likelihood calculation                                                                         |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~lskip                     | int       | 0            | The number of beams skipped in each scan.                                                                                               |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~minimumScore              | float     | 0.0          | The lowest value of the scan matching result                                                                                            |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~srr                       | float     | 0.1          | The mileage error during translation as a translation function (rho/rho)                                                                |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~srt                       | float     | 0.2          | The mileage error during translation as a rotation function (rho/theta)                                                                 |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~str                       | float     | 0.1          | The mileage error during rotation as a translation function(theta/rho)                                                                  |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~stt                       | float     | 0.2          | The mileage error during rotation as a rotation function (theta/theta)                                                                  |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~linearUpdate              | float     | 1.0          | The robot translates a certain distance and processes the laser data once                                                               |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~angularUpdate             | float     | 0.5          | The robot rotates a certain distance and processes the laser data once                                                                  |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~temporalUpdate            | float     | -1.0         | If the latest scan processing is slower than the update, one scan is processed. Turn off time-based updates when the value is negative. |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~resampleThreshold         | float     | 0.5          | Resampling threshold based on Neff                                                                                                      |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~particles                 | int       | 30           | Number of particles in the filter                                                                                                       |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~xmin                      | float     | -100.0       | The initial minimum size of the map in the x direction                                                                                  |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~ymin                      | float     | -100.0       | The initial minimum size of the map in the y direction                                                                                  |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~xmax                      | float     | 100.0        | The initial maximum size of the map in the x direction                                                                                  |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~ymax                      | float     | 100.0        | The initial maximum size of the map in the y direction                                                                                  |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~delta                     | float     | 0.05         | Map resolution                                                                                                                          |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~llsamplerange             | float     | 0.01         | The translation sampling distance of likelihood calculation                                                                             |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~llsamplestep              | float     | 0.01         | The translation sampling step of likelihood calculation                                                                                 |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~lasamplerange             | float     | 0.005        | The angle sampling distance of likelihood calculation                                                                                   |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~lasamplestep              | float     | 0.005        | The angle sampling step of likelihood calculation                                                                                       |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~transform_publish_period  | float     | 0.05         | TF transform publishing period                                                                                                          |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~occ_thresh                | float     | 0.25         | The threshold of raster map occupancy rate                                                                                              |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| ~maxRange                  | float     | ——           | The maximum range of sensor                                                                                                             |
+----------------------------+-----------+--------------+-----------------------------------------------------------------------------------------------------------------------------------------+
