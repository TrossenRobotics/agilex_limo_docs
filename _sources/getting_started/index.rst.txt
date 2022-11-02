===============
Getting Started
===============

.. contents::
    :local:

LIMO Contents
=============

Your LIMO should come with the following:

*   1x LIMO Robot
*   1x Battery
*   1x Charger
*   4x Mecanum Wheels
*   2x Tracks
*   2x Wi-Fi/Bluetooth Antennas
*   1x Allen Wrench
*   3x M3x12mm Screws
*   20x M3x5mm Screws

First Time Use
==============

1.  Install the two antennas to the connectors on top of the LIMO.
2.  Charge the battery until the LED indicator on the charging blocks is a solid green.
3.  Open the rear door on the LIMO and connect the battery to its connector.
4.  Long press the power button on the left side of the robot to turn it on.
5.  Check the battery charge level by observing the indicator on the right side of the robot.

.. list-table::
    :header-rows: 1
    :align: center

    * - Light Status
      - Meaning
    * - Solid Green
      - Sufficient Battery Charge
    * - Flashing Red
      - Low Battery Charge

6.  Check the current steering mode of the robot by observing the color of the lights at the front
    by the latches.

.. table::
    :align: center

    +--------------+-----------------+------------------------------------------+
    | Latch Status | Indicator Color |      Current Steering Mode or Status     |
    +==============+=================+==========================================+
    |      Any     |   Blinking Red  |   Low Battery or Main Controller Alarm   |
    |              +-----------------+------------------------------------------+
    |              |    Solid Red    |         LIMO Stopped Due to Error        |
    +--------------+-----------------+------------------------------------------+
    |   Inserted   |      Yellow     | Four-wheel Differential Drive or Tracked |
    |              +-----------------+------------------------------------------+
    |              |       Blue      |                  Mecanum                 |
    +--------------+-----------------+------------------------------------------+
    |   Released   |      Green      |                 Ackermann                |
    +--------------+-----------------+------------------------------------------+

7.  Open the door on the right side of the robot. You will see two USB ports on the USB hub. Plug
    in a mouse and keyboard.

    .. image:: _images/limo_side_usbhub.png
        :align: center

8.  Connect your robot to your Wi-Fi using the LIMO computer's settings menu under **All Settings**
    > **Network**.

    .. image:: _images/connect_to_network.png
        :align: center

Mobile App Setup
================

1.  Download and install the controller application.

    - iOS: Search for "Nexus" in the AppStore
    - Android: The QR code below takes you to the download link:

        .. image:: _images/app_android.png
            :align: center

2.  Tap on the Bluetooth icon in the upper left to open the connection menu.

    .. image:: _images/app_home.png
        :align: center
        :width: 70%

3.  Select the connection with the same name as the one on the front of your robot.

    .. image:: _images/app_nearby.png
        :align: center
        :width: 70%

4.  Once connected, you will see the battery level of your robot as well as the connection symbol.
    You should also be able to control your robot. Find information on controlling your robot using
    the mobile app in the :doc:`App Operation Guide</operation/app>`.

    .. image:: _images/app_connected.png
        :align: center
        :width: 70%

Setting Up Remote Development
=============================

Instead of having to hook up a mouse and keyboard to your LIMO every single time you want to use
it, you have the option of doing remote development. There are many ways to accomplish this, but we
will present a few options here.

Remote Desktop (NoMachine) Setup
--------------------------------

NoMachine is a remote desktop software developed by the `Luxembourg-based company of the same
name`_. It comes pre-installed on the LIMO for your convenience. The directions for setting up a
connection between your remote computer and the LIMO are below:

.. _`Luxembourg-based company of the same name`: https://www.nomachine.com/about-us

1.  Download and install the version of NoMachine matching your remote computer's OS from the
    `NoMachine download page`_. Follow the instructions for installation.

.. _`NoMachine download page`: https://www.nomachine.com/download

2.  Make sure that your remote computer and your LIMO are on the same Wi-Fi network.

3.  Open NoMachine on your remote computer.

4.  Choose the LIMO's Jetson Nano from the list of connection options.

    .. image:: _images/nomachine_choose_limo.png
        :align: center

5.  Click "Yes" to verify the host authenticity.

    .. image:: _images/nomachine_verify.png
        :align: center

6.  Enter the username ``agilex`` and the password ``agx``. You can choose the save the password to
    the connection file if you'd like.

    .. image:: _images/nomachine_login.png
        :align: center

7.  Click "OK" to proceed through the tips.

8.  You are now able to remote into your LIMO through NoMachine.

    .. image:: _images/nomachine_connected.png
        :align: center

.. _secure-shell-protocol-setup-linux_label:

Linux Secure Shell Protocol (SSH) Setup
---------------------------------------

Basic `secure shell protocol`_ (SSH) allows a user to access command-line interface on a device
from a remote computer.

.. _`secure shell protocol`: https://en.wikipedia.org/wiki/Secure_Shell

.. TODO - do we need to install SSH on the LIMO?

1.  Install the openssh-client package on your remote linux computer.

    .. code-block:: console

        $ sudo apt install openssh-client

2.  Install the openssh-server package on your LIMO.

    .. code-block:: console

        $ sudo apt install openssh-server

3.  Make sure that your remote computer and your LIMO are on the same Wi-Fi network.

4.  SSH into the LIMO from your remote computer with the display forwarding flag ``-X``.

    .. code-block:: console

        # ssh -X username@hostname.local
        $ ssh -X agilex@agilex.local

    .. image:: _images/ssh.png
        :align: center

    .. note::

        If prompted, continue to connect despite not being able to verify the authenticity of the
        host.

    .. note::

        If prompted, enter the password ``agx`` and accept the SSH key.

    .. note::

        The ``-X`` flag indicates to OpenSSH that we want to do display forwarding, meaning that
        OpenSSH will forward graphical application to the client from the server. On the server
        side, X11Forwarding yes must be specified in /etc/ssh/sshd_config. Note that the default is
        no forwarding (some distributions turn it on in their default /etc/ssh/sshd_config), and
        that the user cannot override this setting.

5.  Once logged in to the LIMO, you can open multiple SSH'ed terminals using the command below.

    .. code:: console

        $ gnome-terminal &

    -  Sometimes, this doesn't work. In that case, use the command from `this Ask Ubuntu answer`_.

        .. code:: console

            $ /usr/bin/dbus-launch /usr/bin/gnome-terminal &

.. _`this Ask Ubuntu answer`: https://askubuntu.com/questions/608330/problem-with-gnome-terminal-on-gnome-3-12-2/1235679#1235679

.. Visual Studio Code Remote Development
.. -------------------------------------

.. 1.  At Trossen Robotics, we use Microsoft's VSCode and its Remote - SSH extension (also developed
..     by Microsoft) for simple remote development on the LIMO.

..    -   `Install VSCode`_ for Ubuntu.

..    -    Open VSCode, Press :kbd:`Ctrl` + :kbd:`P` to launch the Quick Open Menu, and run the following command.

..     .. code::

..         ext install ms-vscode-remote.remote-ssh

.. .. _`Install VSCode`: https://code.visualstudio.com/download

.. 2.  In VSCode, press **F1** and run the ``Remote-SSH: Open SSH Host...`` command. Enter the same
..     ``username@hostname.local`` combination you used when opening the SSH connection between your
..     remote computer and the LIMO like ``agilex@nano.local``. If prompted, enter the password
..     ``agx``.

.. 3.  Once connected, use **File > Open Folder**, and select the directory you wish to operate in,
..     i.e. the ``~/agilex_ws`` directory.

.. 4.  Your instance of VSCode is now attached to the LIMO and is open to your development
..     workspace.

.. 5.  You can open terminals in VSCode by pressing :kbd:`Ctrl` + :kbd:`Shift` + :kbd:`\`` or by using
..     **Terminal > New Terminal**.

.. .. note::

..    It is not simple to configure display forwarding using the Remote-SSH extension at the time of
..    writing this guide. To get around this, you can either follow some of the recommendations in
..    `this GitHub Issue`_, or just `ssh into the limo`_ to launch programs with GUIs.

.. .. _`this GitHub Issue`: https://github.com/microsoft/vscode-remote-release/issues/267
.. .. _`ssh into the limo`: `secure-shell-protocol-setup-linux_label`_

.. _getting-started-turning-off-your-limo-label:

Turning Off Your LIMO
=====================

1.  It is a good idea to cleanly turn off the LIMO's onboard computer when you are finished using it.
    To do this, type ``sudo poweroff`` in its terminal and enter the password ``agx``.

2.  Press and hold the LIMO battery's power button until the robot powers off.
