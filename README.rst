==========
samsungctl
==========

samsungctl is a library and a command line tool for remote controlling Samsung
televisions via a TCP/IP connection. It currently supports both pre-2016 TVs as
well most of the modern Tizen-OS TVs with Ethernet or Wi-Fi connectivity.

Dependencies
============

- 
      -q, --quiet         suppress non-fatal output
      -i, --interactive   interactive control
      --host HOST         TV hostname or IP address
      --port PORT         TV port number (TCP)
      --method METHOD     Connection method (legacy or websocket)
      --name NAME         remote control name
      --description DESC  remote control description
      --id ID             remote control id
      --timeout TIMEOUT   socket timeout in seconds (0 = no timeout)

    E.g. samsungctl --host 192.168.0.10 --name myremote KEY_VOLDOWN

The settings can be loaded from a configuration file. The file is searched from
``$XDG_CONFIG_HOME/samsungctl.conf``, ``~/.config/samsungctl.conf``, and
``/etc/samsungctl.conf`` in this order. A simple default configuration is
bundled with the source as `samsungctl.conf <samsungctl.conf>`_.

Library usage
=============

samsungctl can be imported as a Python 3 library:

.. code-block:: python

    import samsungctl

A context managed remote controller object of class ``Remote`` can be
constructed using the ``with`` statement:

.. code-block:: python

    with samsungctl.Remote(config) as remote:
        # Use the remote object

The constructor takes a configuration dictionary as a parameter. All
configuration was closed.
UnhandledResponse  An unexpected response was received.
socket.timeout     The connection timed out.
=================  =======================================

Example program
---------------

This simple program opens and closes the menu a few times.

.. code-block:: python

    #!/usr/bin/env python3

    import samsungctl
    import time

    config = {
        "name": "samsungctl",
        "description": "PC",
        "id": "",
        "host": "192.168.0.10",
        "port": 55000,
        "method": "legacy",
        "timeout": 0,
    }

    with samsungctl.Remote(config) as remote:
        for i in range(10):
            remote.control("KEY_MENU")
            time.sleep(0.5)

Key codes
=========

The list of accepted keys may vary depending on the TV model, but the following
list has some common key codes and their descriptions.

=================  ============
Key code           Description
=================  ============
KEY_POWEROFF       Power off
KEY_UP             Up
KEY_DOWN           Down
KEY_LEFT           Left
KEY_RIGHT          Right
KEY_CHUP           P Up
KEY_CHDOWN         P Down
KEY_ENTER          Enter
KEY_RETURN         Return
KEY_CH_LIST        Channel List
KEY_MENU           Menu
KEY_SOURCE         Source
KEY_GUIDE          Guide
KEY_TOOLS          Tools
KEY_INFO           Info
KEY_RED            A / Red
KEY_GREEN          B / Green
KEY_YELLOW         C / Yellow
KEY_BLUE           D / Blue
KEY_PANNEL_CHDOWN  3D
KEY_VOLUP          Volume Up
KEY_VOLDOWN        Volume Down
KEY_MUTE           Mute
KEY_0              0
KEY_1              1
KEY_2              2
KEY_3              3
KEY_4              4
KEY_5              5
KEY_6              6
KEY_7              7
KEY_8              8
KEY_9              9
KEY_DTV            TV Source
KEY_HDMI           HDMI Source
KEY_CONTENTS       SmartHub
=================  ============

Please note that some codes are different on the 2016+ TVs. For example,
``KEY_POWEROFF`` is ``KEY_POWER`` on the newer TVs.

References
==========

I did not reverse engineer the control protocol myself and samsungctl is not
the only implementation. Here is the list of things that inspired samsungctl.

- http://sc0ty.pl/2012/02/samsung-tv-network-remote-control-protocol/
- https://gist.github.com/danielfaust/998441
- https://github.com/Bntdumas/SamsungIPRemote
- https://github.com/kyleaa/homebridge-samsungtv2016
