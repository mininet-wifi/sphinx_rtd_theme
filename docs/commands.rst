************
Mininet-WiFi Command-Line Interface (CLI) Commands and Attributes
************

Display Options
===================

To see the list of Command-Line Interface (CLI) options, start up a minimal topology and leave it running. Build the Mininet-WiFi:

.. code-block:: console

    $ sudo mn --wifi


Display the options:
===================


.. confval:: help

    Help command

.. confval:: link node1 node2 up/down

    Link Up/Down: For fault tolerance testing, it can be helpful to bring links up and down

.. confval:: node.setAssociation(ap1, intf='node-wlan0')

    Forcing Association

.. confval:: node.setRange(10, intf='node-wlan0') or net.addStation(... range=10)

    Setting Signal Range

.. confval:: node.wintfs[0].range

    Getting Signal Range

.. confval:: node.setAntennaGain(10, intf='node-wlan0') or net.addStation(... antennaGain=10)

    Setting the Antenna Gain

.. confval:: node.wintfs[0].antennaGain

    Getting the Antenna Range

.. confval:: node.setTxPower(10, intf='node-wlan0') or net.addStation(... txpower=10)

    Setting the Transmission Power

.. confval:: node.wintfs[0].power

    Getting the Transmission Power

.. confval:: node.setChannel(1, intf='node-wlan0')

    Setting the Channel

.. confval:: node.wintfs[0].channel

    Getting the Channel

.. confval:: node.setIntfName('newName', 0)

    Setting a new interface name: you can replace `newName` by any name and `0` by the id of the interface. For example: if the original interface is `node-wlan0` the id should by 0 while `node-wlan1` should be 1 and so on.

.. confval:: node.show()

    Showing Nodes
        
.. confval:: node.hide()

    Hiding Nodes

.. confval:: node.set_circle_color('r')  # for red color

    Setting Circle Color

.. confval:: node.setMasterMode(intf='node-wlan0', ssid='new-ssid', channel='1', mode='g')

    Setting Master Mode

.. confval:: node.setManagedMode(intf='node-wlan0')

    Setting Managed Mode

.. confval:: node.setAdhocMode(intf='node-wlan0')

    Setting Adhoc Mode

.. confval:: node.setMeshMode(intf='node-wlan0')

    Setting Mesh Mode

.. confval:: node.setPosition('10,10,0') # x=10, y=10, z=0

    Setting Node Position

.. confval:: node.position

    Getting Node Position

.. confval:: node.stop_()

    Shutting AP down

.. confval:: node.start_()

    Bringing AP up

.. confval:: stop

    Pause the simulation

.. confval:: start

    Continue the simulation

.. code:: xterm node1 node2

    XTerm Display: To display an xterm for sta1 and sta2:
