**************************
SUMO (Simulation of Urban MObility)
**************************


.. image:: https://github.com/mininet-wifi/mininet-wifi.github.io/blob/master/assets/img/sumo.png?raw=true


**Usage**

You can run Sumo with:

.. code:: console

    net.useExternalProgram(program=sumo, port=8813,
                           config_file='map.sumocfg'
                           clients=1,
                           exec_order=0,
                           extra_params=["--start --delay 1000"])

.. Note::

    - Some optional parameters include:
    - **port**: traci server port number
    - **config_file**: sumo's config file
    - **clients**: number of clients. You may want to refer to https://sumo.dlr.de/docs/TraCI/Protocol.html
    - exec_order: order of execution when there are multiple threads
    - extra_params: any other parameter supported by sumo


For your convenience a sample file is available at `/examples/vanet-sumo.py <https://github.com/intrig-unicamp/mininet-wifi/blob/master/examples/vanet-sumo.py>`_.
