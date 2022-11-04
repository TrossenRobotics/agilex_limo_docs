==============
Path Following
==============

.. contents::
    :local:

Overview
========

Structure
=========

Usage
=====

1.  First launch the LiDAR, open a new terminal, and enter the command in the terminal:

    .. code-block::

        $ roslaunch limo_bringup limo_start.launch pub_odom_tf:=false

2.  Launch the navigation function, open a new terminal, and enter the command in the terminal:

    .. code-block::

        $ roslaunch limo_bringup limo_navigation_diff.launch

    .. note::

        If it is Ackermann motion mode, please run

        .. code-block::

            $ roslaunch limo_bringup limo_navigation_ackerman.launch

3.  Launch the path recording function, open a new terminal, and enter the command in the terminal:

    .. code-block::

        $ roslaunch agilex_pure_pursuit record_path.launch

After the path recording is over, terminate the path recording program, and enter the command in
the terminal: :kbd:`Ctrl` + :kbd:`C`.

4.  Launch the path inspection function, open a new terminal, and enter the command in the terminal:

.. note::

    adjust the handle to command mode.

    .. code-block::

        $ roslaunch agilex_pure_pursuit pure_pursuit.launch
