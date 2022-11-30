==============================
Flash LIMO Controller Firmware
==============================

1.  On the LIMO's Jetson Nano, download the latest firmware image from the
    ``limo_software/limo_firmware update`` directory of AgileX's `limo-doc GitHub repository`_. It
    will be named something like ``LimonTestVX.X_Nano``.

.. _`limo-doc GitHub repository`: https://github.com/agilexrobotics/limo-doc

2.  Power off the robot.

3.  When the robot is powered off, press the power button twice to enter the firmware upgrade mode.
    When the power button flashes, it has successfully entered the firmware upgrade mode. After a
    few seconds, the Jetson Nano will start normally.

4.  Make the LimonTest_Nano software executable. Open a terminal and enter the command:

    .. code-block:: bash

        $ chmod +x LimoTestV1.1_Nano
        #          ^^^^^^^^^^^^^^^^^ replace this with the actual name of the firmware

5.  Launch the software and start to upgrade the firmware. Enter the command:

    .. code-block:: bash

        $ ./LimoTestV1.1_Nano

6.  After the software is successfully opened, click the upgrade button, and the displayed screen
    is as shown in the figure below:

    .. image:: _images/gujian_1.png
        :align: center
        :width: 80%

7.  Select the corresponding serial port. This will typically be ``ttyTHS1``. Click **Open Serial**
    to open the serial port, and then click **Load Firmware File** to select the firmware to be
    upgraded.

    .. image:: _images/gujian_2.png
        :align: center
        :width: 80%

    .. image:: _images/gujian_3.png
        :align: center
        :width: 80%

8.  Select the firmware information in the firmware list, and then click the **Start Upgrade**
    button to start the firmware upgrade.

    .. image:: _images/gujian_4.png
        :align: center
        :width: 80%

9.  After the upgrade is successful, click the **Close Serial** button to close the serial port.
