************
Mininet-WiFi Command-Line Interface (CLI) Commands and Attributes
************

Display Options
===================

To see the list of Command-Line Interface (CLI) options, start up a minimal topology and leave it running. Build the Mininet-WiFi:

:console:
    $ sudo mn --wifi


Display the options:
===================

:console:
    mininet-wifi> help


Python Interpreter
===================
If the first phrase on the Mininet-WiFi command line is py, then that command is executed with Python. This might be useful for extending Mininet-WiFi, as well as probing its inner workings. Each station, ao, and controller has an associated Node object.

At the Mininet CLI, run:
:console:
    mininet-wifi> py 'hello ' + 'world'


Print the accessible local variables:
:console:
    mininet-wifi> py locals()


Link Up/Down
===================
For fault tolerance testing, it can be helpful to bring links up and down.

To disable both halves of a virtual ethernet pair:
:console:
    mininet-wifi> link ap1 sta1 down


You should see an OpenFlow Port Status Change notification get generated. To bring the link back up:
:console:
    mininet-wifi> link sta1 ap1 up


Forcing Association
===================

You can force the association with an AP either by using iw tool:
:console:
    mininet-wifi> sta1 iw dev sta1-wlan0 connect new-ssid


or by using the Mininet-WiFi's API:
:console:
    mininet-wifi> py sta1.setAssociation(ap1, intf='sta1-wlan0')


Setting Signal Range
===================
You can set the Signal Range when the node is being created:
:console:
    net.addStation(... range=10)


or at runtime:
:console:
    mininet-wifi> py sta1.setRange(10, intf='sta1-wlan0')


and confirm the new value with:
:console:
    mininet-wifi> py sta1.wintfs[0].range


Keep in mind that if the signal range changes, txpower will also change.

Setting Antenna Gain
===================
You can set the Antenna Gain when the node is being created:
:console:
    net.addStation(... antennaGain=10)


or at runtime:
:console:
    mininet-wifi> py ap1.setAntennaGain(10, intf='ap1-wlan1')


and confirm the new value with:
:console:
    mininet-wifi> py sta1.wintfs[0].antennaGain


Setting Tx Power
===================

You can set the Tx Power either by iw tool (for txpower = 10):
:console:
    mininet-wifi> sta1 iw dev sta1-wlan0 set txpower fixed 1000


or by using the Mininet-WiFi's API:
:console:
    net.addStation(... txpower=10)


as well as at runtime:
:console:
    mininet-wifi> py ap1.setTxPower(10, intf='ap1-wlan1')


Confirming the new value:
:console:
    mininet-wifi> py ap1.wintfs[0].txpower


Setting Channel
===================
You can set the channel either by iw tool:
### if the node is AP:
:console:
    mininet-wifi> ap1 hostapd_cli -i ap1-wlan1 chan_switch 1 2412

### if the node is working in mesh mode:
:console:
    mininet-wifi> sta1 iw dev sta1-mp0 set channel 1

### if the node is working in adhoc mode:

:console:
    mininet-wifi> sta1 iw dev sta1-wlan0 ibss leave
    mininet-wifi> sta1-wlan0 ibss join adhocNet 2412 02:CA:FF:EE:BA:01

or by using the Mininet-WiFi's API:
:console:
    mininet-wifi> py sta1.setChannel(1, intf='ap1-wlan1')


Confirming the new value:
:console:
    mininet-wifi> py sta1.wintfs[0].channel


Renaming the Interface Name
===================

You can rename the network interface name with:
:console:
    sta1.setIntfName('newName', 0)


You can replace `newName` by any name and `0` by the id of the interface. For example: if the original interface is `sta1-wlan0` the id should by 0 while `sta1-wlan1` should be 1 and so on.

Showing and Hiding Nodes
===================

You can hide the node with:
:console:
    sta1.hide()


You can show the node again with:
:console:
    sta1.show()


Setting Circle Color
===================
You can set the signal range - circle - color with:
:console:
    sta1.set_circle_color('r')  # for red color


Setting the Operation Mode
===================

### Master
:console:
    sta1.setMasterMode(intf='sta1-wlan0', ssid='ap1-ssid', channel='1', mode='g')


### Managed
:console:
    ap1.setManagedMode(intf='ap1-wlan1')


### Adhoc
:console:
    sta1.setAdhocMode(intf='sta1-wlan0')


### Mesh
:console:
    sta1.setMeshMode(intf='sta1-wlan0')


Setting the Node Position
===================
:console:
    mininet-wifi> py sta1.setPosition('10,10,0') # x=10, y=10, z=0


Confirming the position:
:console:
    mininet-wifi> py sta1.position


Shutting AP down
===================
You can shutdown the AP with:
:console:
    mininet-wifi> py ap1.stop_()

and bring it up again with:

:console:
    mininet-wifi> py ap1.start_()


Stopping the Simulation
===================
Considering that you have some simulation with mobility running you can stop it with:
:console:
    mininet-wifi> stop


And run it again with:

:console:
    mininet-wifi> start


XTerm Display
===================
To display an xterm for sta1 and sta2:

:console:
    mininet-wifi> xterm sta1 sta2