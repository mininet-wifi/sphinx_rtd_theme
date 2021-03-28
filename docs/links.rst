************
Creating Links
************

Mesh Link
===================

A mesh link with IEEE 802.11s can be created with the method below:

.. code:: python

    net.addLink(sta1, cls=mesh, intf='sta1-wlan0', ssid='meshNet', channel=5)


The command above crates a mesh link from `sta1-wlan0` through iw. Please refer to the iw documentation on how to add a new virtual interface for further information.

If you want to create multiple virtual mesh interfaces you can repeat the method call by setting `vIface=True` as follows.

.. code:: python

    net.addLink(sta1, cls=mesh, ssid='meshNet', vIface=True,
                intf='sta1-wlan0', channel=5, ht_cap='HT40+')


Ad hoc Link
===================

A adhoc link can be created with the method below:

.. code:: python

    net.addLink(sta1, cls=adhoc, intf='sta1-wlan0', ssid='adhochNet', channel=5)
