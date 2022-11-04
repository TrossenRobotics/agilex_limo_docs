==============
Voice Commands
==============

.. contents::
    :local:

Overview
========

Control the robot using your voice, commanding it to move ahead, move back, turn right, and turn
left by saying "ahead", "back", "right" and "left".

Structure
=========

Usage
=====

.. note::

    Use :kbd:`Ctrl` + :kbd:`C` command to end all the processes before running the below commands.

1.  Start the LIMO. Open a new terminal, and enter the command:

    .. code-block::

        $ roslaunch limo_base limo_base.launch

2.  Set the controller to Command Mode once the node is launched.

.. TODO(lsinterbotix): ?

3.  Launch the voice control node. After launching, the interface shown in the figure below will
    appear. Enter ``1`` to start the voice recording mode and control the robot. Enter ``q`` to
    quit.

    .. code-block::

        $ rosrun voice voice_ctr_node.py

    .. image:: _images/voice_control.png
        :align: center
