==============
Speech To Text
==============

.. contents::
    :local:

Overview
========

Sound is recorded to a .wav file using the Nano's external sound card. Voice recognition is done
with the voice library `pocketsphinx`_.

.. _`pocketsphinx`: https://github.com/cmusphinx/pocketsphinxs

Structure
=========

Usage
=====

.. note::

    Use :kbd:`Ctrl` + :kbd:`C` command to end all the processes before running the below commands.

1.  Start a ROS master. Open a new terminal, and enter the command:

    .. code-block::

        $ roscore

1.  Run the voice recording script from the voice package. When “recording” appears in the
    terminal, start to record the voice. After 3 seconds, the recording is complete, and “Done”
    will appear on the terminal. The .wav file will be saved to a file in the current directory
    called ``output.wav``. Open a new terminal, and enter the command:

    .. code-block::

        $ rosrun voice demo_record_voice.py

    .. image:: _images/voice_record.png
        :align: center

2.  Once the recording is saved, run the speech recognition script. The recognized text will be
    printed to the terminal. Using the same terminal, enter the command:

    .. code-block::

        $ rosrun voice demo_voice2word.py output.wav

    .. image:: _images/speech_to_text.png
        :align: center
