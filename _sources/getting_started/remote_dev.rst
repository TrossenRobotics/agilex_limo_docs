=============================
Setting Up Remote Development
=============================

Instead of having to hook up a mouse and keyboard to your LIMO every single time you want to use
it, you have the option of doing remote development. There are many ways to accomplish this, but we
will present a few options here.

Remote Desktop (NoMachine) Setup
================================

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
=======================================

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
.. =====================================

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
