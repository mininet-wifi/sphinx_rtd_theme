**************************
Manet Routing Protocols
**************************

Mininet-WiFi will basically support any Protocol supported by Linux Systems. However, the way that such protocols work can be automated.

For example, we have added B.A.T.M.A.N, OLSR and BABEL in `manetRoutingProtocols.py <https://github.com/intrig-unicamp/mininet-wifi/blob/master/mn_wifi/manetRoutingProtocols.py>`_ and they can be used as below:

.. code:: console

    net.addLink(sta1, cls=adhoc, intf='sta1-wlan0',
                ssid='adhocNet', proto=olsr,
                mode='g', channel=5, ht_cap='HT40+')


You can replace _olsr_ by _batman_ and/or _babel_.

Arguments can be set with `proto_args`. For example, if you want to set the `hello interval` you can consider the excerpt below:

.. code:: console

    net.addLink(sta1, cls=adhoc, intf='sta1-wlan0',
                ssid='adhocNet', proto=olsr,
                proto_args='-hint 10',
                mode='g', channel=5, ht_cap='HT40+')


Multiple arguments can also be passed in `proto_args`.

The list of parameters can be found with the protocol you are using. For example, we found `-hint` by issuing the `olsrd --help` command.


Installing
===================

**BATMAN**

.. code:: console

    sudo util/install.sh -B


**BABEL**

.. code:: console

    sudo util/install.sh -E


**OLSR**

.. code:: console

    sudo util/install.sh -O