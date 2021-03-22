**************************
6LoWPAN
**************************


6LoWPAN is supported by Mininet-WiFi thanks to the fakelb and mac802154_hwsim module. Both modules have been developed to support 6lowpan, but mac802154_hwsim (which is supported from Linux Kernel version 4.18) is gradually replacing fakelb.

You can find an example for 6LoWPAN at `examples/6LoWPan.py <https://github.com/intrig-unicamp/mininet-wifi/blob/master/examples/6LoWPan.py>`_. However, you first need to install iwpan tool with:

.. code:: console

    ~/mininet-wifi$ sudo util/install.sh -6


6LoWPan.py consists of three sensors and you can interact with these nodes in the same fashion as stations and hosts. For example:

.. code:: console

    ~/mininet-wifi$ sudo python examples/6LoWpan.py
    mininet-wifi> sensor1 ping6 -c1 2001::2
    PING 2001::2(2001::2) 56 data bytes
    64 bytes from 2001::2: icmp_seq=1 ttl=64 time=0.221 ms

    --- 2001::2 ping statistics ---
    1 packets transmitted, 1 received, 0% packet loss, time 0ms
    rtt min/avg/max/mdev = 0.221/0.221/0.221/0.000 ms


You can also use iwpan tool:

.. code:: console

    mininet-wifi> sensor1 iwpan dev sensor1-wpan0 info
    Interface sensor1-wpan0
        ifindex 5
        wpan_dev 0x2
        extended_addr 0x466abb26bbd7f534
        short_addr 0xffff
        pan_id 0xbeef
        type node
        max_frame_retries 3
        min_be 3
        max_be 5
        max_csma_backoffs 4
        lbt 0
        ackreq_default 0


Alternatively, you can --help for more more information about the features supported by iwpan.

.. code:: console

    mininet-wifi> sensor1 iwpan --help


**Demo Video**

`https://www.youtube.com/watch?v=LKTMiUyB6Lk <https://www.youtube.com/watch?v=LKTMiUyB6Lk>`_