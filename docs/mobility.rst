**************************
Mobility Models
**************************

Mininet-WiFi supports the following mobility models:

- RandomWalk
- TruncatedLevyWalk
- RandomDirection
- RandomWayPoint
- GaussMarkov
- ReferencePoint
- TimeVariantCommunity

How to configure mobility models?
===================

The mobility models can be configured with `net.setMobilityModel()`. For example:

.. code:: console

    net.setMobilityModel(time=0, model='RandomDirection',
                         max_x=100, max_y=100, seed=20)


- **time**: time (in seconds) in which the mobility is started
- **model**: mobility model
- **max_x**: maximum x value
- **min_x**: minimum x value
- **seed**: defines a starting value for a pseudo random sequence


In addition, you may want to consider some parameters that be can be used when you add some node:

Random Walk
===================

.. code:: console

    sta1 = net.addStation( ..., min_x=10, max_x=20, min_y=10, max_y=20, constantDistance=1, constantVelocity=1 )

- **min_x** = Minimum X position # default=0
- **max_x** = Maximum X position # default=100
- **min_y** = Minimum Y position # default=0
- **max_y** = Maximum Y position # default=100
- **constantDistance** = the value for the constant distance traveled in each step. Default is 1.0.
- **constantVelocity** = the value for the constant node velocity. Default is 1.0


Random Direction
===================

.. code:: console

    sta1 = net.addStation( ..., min_x=10, max_x=20, min_y=10, max_y=20, min_v=1, max_v=2 )


- **min_x** = Minimum X position # default=0
- **max_x** = Maximum X position # default=100
- **min_y** = Minimum Y position # default=0
- **max_y** = Maximum Y position # default=100
- **min_v** = Minimum value for node velocity
- **max_v** = Maximum value for node velocity


Random Way Point
===================

.. code:: console

    sta1 = net.addStation( ..., min_x=10, max_x=20, min_y=10, max_y=20, min_v=1, max_v=2, min_wt=0, max_wt=100 )

- **min_x** = Minimum X position # default=0
- **max_x** = Maximum X position # default=100
- **min_y** = Minimum Y position # default=0
- **max_y** = Maximum Y position # default=100
- **min_v** = Minimum value for node velocity # default=5
- **max_v** = Maximum value for node velocity # default=5
- **min_wt** = Minimum wait time # default=0
- **max_wt** = Maximum wait time # default=100

Gaus Markov
===================

.. code:: console

    sta1 = net.addStation( ..., min_x=10, max_x=20, min_y=10, max_y=20 )


- **min_x** = Minimum X position # default=0
- **max_x** = Maximum X position # default=100
- **min_y** = Minimum Y position # default=0
- **max_y** = Maximum Y position # default=100


Custom Mobility
===================

You can also define custom mobility. To do so, you may want to refer to `examples/mobility.py <https://github.com/intrig-unicamp/mininet-wifi/blob/master/examples/mobility.py) and [`examples/replaying/replayingMobility.py`](https://github.com/intrig-unicamp/mininet-wifi/blob/master/examples/replaying/replayingMobility.py>`_ for more information.


Handover association mechanisms
===================

Two mechanisms can be considered:

- Least Loaded First (llf)
- Strongest Signal First (ssf)

You can use them when the mobility is started. For example:

.. code:: console

    net.startMobility(time=0, ac_method='ssf')
    net.stopMobility(time=10)

or

.. code:: console

    net.setMobilityModel(... ac_method='ssf')


However, if you want to work with a seamless handover you may want to refer to bgscan (see `examples/handover_bgscan.py <https://github.com/intrig-unicamp/mininet-wifi/blob/master/examples/handover_bgscan.py>`_

In scenarios where there is no mobility you can enable `ac_method` within `Mininet_wifi()`. For example:


.. code:: console

    Mininet_wifi(... ac_method='ssf')
