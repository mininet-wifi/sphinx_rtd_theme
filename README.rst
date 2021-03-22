**************************
Introduction
**************************


Learn more about Mininet-WiFi and SDN with :ref:`The Mininet-WiFi Book <https://mininet-wifi.github.io/book>`


Mininet-WiFi is a fork of the Mininet SDN network emulator and extended the functionality of Mininet by adding virtualized WiFi Stations and Access Points based on the standard Linux wireless drivers and the 80211_hwsim wireless simulation driver. This means that new classes has been added in order to support the addition of these wireless devices in a Mininet network scenario and to emulate the attributes of a mobile station such as position and movement relative to the access points.

Mininet-WiFi extends the Mininet code base by adding or modifying classes and scripts. So, Mininet-WiFi adds new functionality and still supports all the normal SDN emulation capabilities of the standard Mininet network emulator.

Requirements
===================

Although we recommend you to use latest LTS version of Ubuntu, Mininet-WiFi should work fine in any Ubuntu Distribution from 14.04.

Architecture and Components
===================

The main components that make part of the development of Mininet-WiFi are illustrated in the figure below. In the kernel-space the module mac80211_hwsim is responsible for creating virtual Wi-Fi interfaces, important for stations and access points. Still in the kernel-space, MLME (Media Access Control Sublayer Management Entity) is realized in the stations side, while in the user-space the hostapd is responsible for this task in the AP side.

.. image:: https://github.com/mininet-wifi/mininet-wifi.github.io/blob/master/assets/img/components.png?raw=true

Mininet-WiFi also uses a couple utilities such as iw, iwconfig e o wpa_supplicant. The first two are used for interface configuration and for getting information from wireless interfaces and the last one is used with Hostapd, in order to support WPA (Wi-Fi Protected Access), among other things. Besides them, another fundamental utility is TC (Traffic Control). The TC is an user-space utility program used to configure the Linux kernel packet scheduler, responsible for controlling the rate, delay, latency and loss, applying these attributes in virtual interfaces of stations and APs, representing with higher fidelity the behavior of the real world.

Figure below depicts the components and connections in a simple topology with two stations (or hosts) created with Mininet-WiFi, where the newly implemented components (highlighted in gray) are presented along the original Mininet building blocks. Although stations are equipped with a wireless interface by default they are able to connect with access points through wired links (veth pairs) as well.


.. image:: https://github.com/mininet-wifi/mininet-wifi.github.io/blob/master/assets/img/arch.png?raw=true

Linux OS network namespaces interconnected through virtual Ethernet (veth) pairs. The wireless interfaces to virtualize WiFi devices work on master mode for access points and managed mode for stations.

**Stations**: Are devices that connect to an access point through authentication and association. In our implementation, each station has one wireless card (staX-wlan0 - where X shall be replaced by the number of each station). Since the traditional Mininet hosts are connected to an access point, stations are able to communicate with those hosts.

**Access Points**: Are devices that manage associated stations. Virtualized through hostapd daemon and use virtual wireless interfaces for access point and authentication servers. Mininet-WiFi currently includes support for the user space reference implementations and Open vSwitch in kernel and user space modes. Mininet-WiFi used to support the OpenFlow 0.8.9 kernel reference implementation (--ap kernel) but that is now obsolete and has largely been replaced with Open vSwitch.
The command line options are --ap user (the same as UserAP) and --ap ovsk (the same as OVSAP or OVSKernelAP) for the user reference and Open vSwitch kernel aps, respectively.
You can also install the :ref:`CPqD BOFUSS <https://github.com/CPqD/ofsoftswitch13>` ap using install.sh -3f; it will replace the Stanford reference switch, i.e. --ap user and UserAP. See below for an example of using it.

Both stations and access points use cfg80211 to communicate with the wireless device driver, a Linux 802.11 configuration API that provides communication between stations and mac80211. This framework in turn communicates directly with the WiFi device driver through a netlink socket (or more specifically nl80211) that is used to configure the cfg80211 device and for kernel-user-space communication as well.

Wireless Medium Emulation
===================

Mininet-WiFi relies on two approaches for simulating the wireless medium: tc and wmediumd.

Traffic Control (TC)
===================

Tc (traffic control) is the user-space utility program used to configure the Linux kernel packet scheduler. Used to configure Traffic Control in the Linux kernel, Traffic Control consists of the following:

- Shaping: When traffic is shaped, its rate of transmission is under control. Shaping may be more than lowering the available bandwidth - it is also used to smooth out bursts in traffic for better network behaviour. Shaping occurs on egress.
- Scheduling: By scheduling the transmission of packets it is possible to improve interactivity for traffic that needs it while still guaranteeing bandwidth to bulk transfers. Reordering is also called prioritizing, and happens only on egress.
- Policing: Where shaping deals with transmission of traffic, policing pertains to traffic arriving. Policing thus occurs on ingress.
- Dropping: Traffic exceeding a set bandwidth may also be dropped forthwith, both on ingress and on egress.


The aforementioned properties have been used to apply values for bandwidth, loss, latency and delay in Mininet-WiFi. Tc was the first approach adopted in Mininet-WiFi for simulating the wireless medium.

Intermediate Functional Block (IFB) Devices
===================

There are two modes of traffic shaping: ingress and egress. Ingress handles incoming traffic and egress outgoing traffic. Linux does not support shaping/queuing on ingress, but only policing. Therefore IFB exists, which we can attach to the ingress queue while we can add any normal queuing like as egress queue on the IFB device.AP

Intermediate Functional Block (IFB) is an alternative to tc filters for handling ingress traffic, by redirecting it to a virtual interface and treat is as egress traffic. IFB is supported by setting up ifb=True in Mininet_wifi() class. Further information about IFB is available at :ref:`IFB <http://shorewall.net/traffic_shaping.htm#IFB>`

If you want to enable IFB in Mininet-WiFi you need to set IFB = True within _Mininet_wifi()_:

.. code:: console
    net = Mininet_wifi(... ifb=True)


Wmediumd
===================

The kernel module mac80211_hwsim uses the same virtual medium for all wireless nodes. This means all nodes are internally in range of each other and they can be discovered in a wireless scan on the virtual interfaces. Mininet-WiFi simulates their position and wireless ranges by assigning stations to other stations or access points and revoking these wireless associations. If wireless interfaces should be isolated from each other (e.g. in adhoc or mesh networks) a solution like wmediumd is required. It uses a kind of a dispatcher to permit or deny the transfer of packets from one interface to another.

Traffic control versus Wmediumd
===================

Wmediumd has been shown to be the best approach for the simulation of the wireless medium. Some advantages include:

- It isolates the wireless interfaces from each other
- wmediumd implements backoff algorithm. TC relies only in FIFO queue discipline.
- It decides when the association has to be evoked based on the signal level
- Values for bandwidth, loss, latency and delay are applied relying in a matrix. This matrix implements an option to determine PER (packet error rate) with outer matrix defined in IEEE 802.11ax. The matrix is defined in Appendix 3 of :ref:`11-14-0571-12 TGax Evaluation Methodology <https://mentor.ieee.org/802.11/dcn/14/11-14-0571-12-00ax-evaluation-methodology.docx>`.
- We highly recommend wmediumd for both adhoc and wireless mesh networks.
