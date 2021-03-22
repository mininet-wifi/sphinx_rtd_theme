************
Advanced Startup Options
************

Run a Regression Test
You do not need to drop into the CLI; Mininet-WiFi can also be used to run self-contained regression tests.

Run a regression test:

.. code:: console
    $ sudo mn --wifi --test pingpair


This command created a minimal topology, started up the OpenFlow reference controller, ran an all-pairs-ping test, and tore down both the topology and the controller.

Another useful test is iperf (give it about 10 seconds to complete):

.. code:: console
    $ sudo mn --wifi --test iperf

This command created the same Mininet-WiFi, ran an iperf server on one host, ran an iperf client on the second host, and parsed the bandwidth achieved.

The default topology is a single ap connected to two stations. You could change this to a different topo with --topo, and pass parameters for that topology’s creation. For example, to verify all-pairs ping connectivity with one ap and three stations:

Run a regression test:
.. code:: console
    $ sudo mn --wifi --test pingall --topo single,3


Adjustable Verbosity
===================

The default verbosity level is info, which prints what Mininet-WiFi is doing during startup and teardown. Compare this with the full debug output with the -v param:

$ sudo mn --wifi -v debug
.. code:: console
    mininet> exit

Lots of extra detail will print out. Now try output, a setting that prints CLI output and little else:

.. code:: console
    $ sudo mn --wifi -v output
    mininet> exit


Plotting Graph
===================

You need to call `net.plotGraph()`. See sample files at :ref:`examples<https://github.com/intrig-unicamp/mininet-wifi/tree/master/examples>` for your convenience.

<a id="line-styles"></a>
### [Customizing the line style](#line-styles)


When you create a link between two APs a solid line between the two APs is created. However, if you wish to customize the line style you can do as follows:

.. code:: console
    net.addLink(ap1, ap2, ls='.')


The list of line styles supported by Mininet-WiFi is the same that matplotlib supports.

Client Isolation
===================


By default, stations associated with the same access point can communicate with each other without OpenFlow rules. If you want to enable OpenFlow in such case, you need to enable the client
isolation. You can either try
.. code:: console
    sudo mn --wifi --client-isolation

or take :ref:`examples/simplewifitopology.py<https://github.com/intrig-unicamp/mininet-wifi/blob/master/examples/simplewifitopology.py>` as reference.

Client isolation can be used to prevent low-level bridging of frames between associated stations in the BSS. By default, this bridging is allowed.

You may also want to refer to the OpenFlow spec.
:ref:`B.6.3 IN PORT Virtual Port<https://www.opennetworking.org/images/stories/downloads/sdn-resources/onf-specifications/openflow/openflow-switch-v1.5.0.noipr.pdf>`
)
**The behavior of sending out the incoming port was not clearly defined in earlier versions of the specification. It is now forbidden unless the output port is explicitly set to OFPP_IN_PORT virtual port (0xfff8) is set. The primary place where this is used is for wireless links, where a packet is received over the wireless interface and needs to be sent to another host through the same interface. For example, if a packet needed to be sent to all interfaces on the switch, two actions would need to be specified: ”actions=output:ALL,output:IN PORT”.**

Multiple Wireless Network Interfaces
===================

Wireless nodes can have multiple wireless interfaces. The wlans parameter Multiple Wirelessallows you to add many interfaces on a single node. For example, let’s take the code below:
.. code:: console
    sta1 = net.addStation('sta1', wlans=2)


wlans=2 means that two wireless interfaces will be creted for sta1. APs can have multiple wireless interfaces as well, however, they deserve a particular attention. For example, let’s take the code below:
.. code:: console
    ap1 = net.addAccessPoint('ap1', wlans=2, ssid=['ssid1','ssid2'], mode='g', channel='1')


You have to define two SSIDs separated by comma in array style. If you do not want two SSIDs for some reason, you can do like below:

.. code:: console
    ap1 = net.addAccessPoint('ap1', wlans=2, ssid=['ssid1',''], mode='g', channel='1')

or even
.. code:: console
    ap1 = net.addAccessPoint('ap1', wlans=2, ssid=ssid1, mode='g', channel='1')


Multiple SSIDs over a Single AP
===================
It is very common for an organization to have multiple SSIDs in their wireless network for various purposes, including: (i) to provide different security mechanisms such as WPA2-Enterprise for your employees and an “open” network with a captive portal for guests; (ii) to split bandwidth among different types of service; or (iii) to reduce costs by reducing the amount of physical access points. In Mininet-WiFi, an unique AP supports up to 8 different SSIDs (limitation imposed by mac80211_hwsim). Multiple SSIDs can be configured as below:
.. code:: console
    ap1 = net.addAccessPoint('ap1',  vssids='ssid1,ssid2,ssid3,ssid4', ssid='ssid', mode='g', channel='1')


Network Address Translator (NAT)
===================

You can add a NAT to the Mininet-WiFi network by calling _net.addNAT()_, as illustrated in the code below.

.. code:: python
    #!/usr/bin/python

    "Example to create a Mininet-WiFi topology and connect it to the internet via NAT"

    from mininet.node import Controller
    from mininet.log import setLogLevel, info
    from mn_wifi.cli import CLI_wifi
    from mn_wifi.net import Mininet_wifi


    def topology():

        "Create a network."

        net = Mininet_wifi(controller=Controller)

        info("*** Creating nodes\n")
        ap1 = net.addAccessPoint('ap1', ssid='new-ssid', mode='g', channel='1', position='10,10,0')
        sta1 = net.addStation('sta1', position='10,20,0')
        c1 = net.addController('c1', controller=Controller)

        info("*** Configuring wifi nodes\n")
        net.configureWifiNodes()

        info("*** Starting network\n")
        net.build()
        net.addNAT(name='nat0', linkTo='ap1', ip='192.168.100.254').configDefault()
        c1.start()
        ap1.start([c1])

        info("*** Running CLI\n")
        CLI_wifi(net)

        info("*** Stopping network\n")
        net.stop()


    if __name__ == '__main__':
        setLogLevel('info')
        topology()


According to the code below, _addNAT_ creates a Node named _nat0_ linked with _ap1_. The IP 192.168.100.254 will be assigned to _nat0_ and this is the default gateway assigned to the all nodes that make up the network topology (only _sta1_ in our case).

.. code:: console
    net.addNAT(name='nat0', linkTo='ap1', ip='192.168.100.254').configDefault()


Authentication
===================

Mininet-WiFi supports WEP, WPA, WPA2 and WPA3. A sample file is available for your convenience at :ref:`examples/authentication<https://github.com/intrig-unicamp/mininet-wifi/blob/master/examples/authentication.py>`

**note**: OVS does not support WPA in the kernel space. The only way to make OVS work with WPA is by setting datapath = "user" as below:

.. code:: console
    ap1 = net.addAccessPoint('ap1', .... datapath='user')


Background Scanning
===================

wpa_supplicant behavior for background scanning can be specified by configuring a bgscan module. These modules are responsible for requesting background scans for the purpose of roaming within an ESS (i.e., within a single network block with all the APs using the same SSID). You can find more information about bgscan at :ref:`wpa_supplicant.conf<https://w1.fi/cgit/hostap/plain/wpa_supplicant/wpa_supplicant.conf>`


Energy Consumption
===================
We have started an implementation of an :ref:`Energy Consumption model<https://github.com/intrig-unicamp/mininet-wifi/blob/master/mn_wifi/energy.py>` where you can set the voltage to the node. In :ref:`battery.py<https://github.com/intrig-unicamp/mininet-wifi/blob/master/examples/battery.py>` you can get the total of energy consumption with `sensor1.wintfs[0].consumption`. Please note that this is only an initial implementation and contributions are most than welcome.


Building Topologies with GUI
===================

.. image:: https://github.com/mininet-wifi/mininet-wifi.github.io/blob/master/assets/img/miniedit.png?raw=true

You can run Miniedit from the __examples__ directory. For example:

.. code:: console
    ~/mininet-wifi$ sudo python examples/miniedit.py



Socket Communication
===================

The socket communication allows you to access methods implemented in Mininet-WiFi as well as send commands from APs, stations, cars, etc. You only need to start the socket server and access it through the socket client.

A sample file is available at :ref:`examples/socket_server.py<https://github.com/intrig-unicamp/mininet-wifi/blob/master/examples/socket_server.py>`

Some of the information you can get from the nodes include:
* **position** - get.node.position
* **channel** - get.node.wintfs[0].channel
* **mode** - get.node.wintfs[0].mode
* **rssi** - get.node.wintfs[0].rssi
* **txpower** - get.node.wintfs[0].txpower

Some of the information you can set to the nodes include:
* **position** = set.node.setPosition("10,10,0")
* **txpower** = set.node.setTxPower(10, intf=sta1-wlan0)
* **range** = set.node.setRange(100, intf=sta1-wlan0)
* **roam** = set.node.roam(bssid, intf=sta1-wlan0)


Demo Video
===================
* :ref:`https://www.youtube.com/watch?v=k69t9Xkb0nU<https://www.youtube.com/watch?v=k69t9Xkb0nU>`
