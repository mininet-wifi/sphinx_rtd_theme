**************************
SUMO (Simulation of Urban MObility)
**************************


.. image:: https://github.com/mininet-wifi/mininet-wifi.github.io/blob/master/assets/img/sumo.png?raw=true


**Usage**

You can run Sumo with:

.. code:: console

    net.useExternalProgram(program=sumo, port=8813,
                           config_file='map.sumocfg'
                           clients=1)

.. Note::

    - **port**: traci server port number
    - **config_file**: the sumo's config. Currently available at mn_wifi/sumo/data/
    - **clients**: number of clients. You may want to refer to https://sumo.dlr.de/docs/TraCI/Protocol.html


For your convenience a sample file is available at `/examples/vanet-sumo.py <https://github.com/intrig-unicamp/mininet-wifi/blob/master/examples/vanet-sumo.py>`_.
