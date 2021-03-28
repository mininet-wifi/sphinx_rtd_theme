**************************
FAQ
**************************

I do not have mac80211_hwsim. How can I install it?
===================

Run the following:

.. code:: console

    sudo apt install linux-image-extra-`uname -r`

How can I control my Mininet-WiFi nodes remotely?
===================

It's trivial to control Mininet-WiFi nodes from the CLI or from within a Python script running locally, but what if you want some other process or even another computer on your LAN to be able to control your Mininet network remotely?

Well, there are lots of ways to do this. One idea is that anything you can do in Python, you can do in Mininet-WiFi, and it's often very easy to do so. For example, there are all sorts of frameworks available for any kind of messaging you can imagine.

Another easy way to control Mininet-WiFi nodes is to use the util/m script. For example:

.. code:: console

    ~/mininet-wifi$ util/m sta1 ifconfig

Another way is by using sockets. See `https://mininet-wifi.github.io/part4 <https://mininet-wifi.github.io/part4>`_

The mode N supports booth 2.4 and 5Ghz. How can I make a choice between 2.4 or 5Ghz?
===================

You have to set band=2.4 or band=5 when you add an AP. For example:

.. code:: console

    net.addAccessPoint(... band='2.4')


Is it possible to create a wired link between station and ap?
===================

Yes. When you add a link between station and ap you have to add cls=TCLink, for example:

.. code:: console

    from mininet.link import TCLink
    ...
    net.addLink(sta1, ap1, cls=TCLink)


Can stations ping APs?
===================

First of all we invite you to read `https://github.com/mininet/mininet/wiki/FAQ#assign-macs <https://github.com/mininet/mininet/wiki/FAQ#assign-macs>`_. However, If you really want stations to ping APs, you may want to set IP address to the wireless network interface and (a) if _OVSAP_: set datapath='user' when you add the AP; or (b) use _UserAP_ with [BOFUSS](https://github.com/CPqD/ofsoftswitch13) and set inNamespace=True when you add the AP.

How to uninstall Mininet-WiFi?
===================

.. code:: console

    sudo rm -rf /usr/local/bin/mn /usr/local/bin/mnexec /usr/local/lib/python*/*/*mininet* /usr/local/bin/ovs-* /usr/local/sbin/ovs-*


What does the warning message about the signal range mean?
===================


.. note:: If you define the signal range for a node you can get the following message:
        **WARNING: The signal range for sta1-wlan0 should be changed to 35**


This message means that you have defined a signal range that is not supported by the propagation model you are using. To fix this you have to either modify the parameters supported by the propagation model in order to support smaller signal ranges (e.g. exponent and system loss) or define the minimum supported by the propagation model. According to the message displayed above, if you define a signal level less than 35, this will be useful only for visualization purposes, as it will actually be transmitting up to 35m.


How can I upgrade my Mininet-WiFi/get the most recent source code?
===================


.. code:: console

    ~/mininet-wifi$ git pull
    ~/mininet-wifi$ sudo make install
    
.. note:: You can confirm that with the `git log` command.
