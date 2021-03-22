**************************
Containernet
**************************

Containernet is a fork of Mininet that allows to use Docker containers as Mininet hosts as well as Mininet-WiFi stations.
This enables interesting functionalities to built networking/cloud testbeds. The integration is done by subclassing the original Host/Station classes.


Get started with Containernet and Mininet-WiFi
===================

Follow these below for installing Containernet with Wi-Fi capabilities:

.. code:: console

    ~$ sudo apt-get install ansible git aptitude
    ~$ git clone https://github.com/ramonfontes/containernet.git
    ~$ cd containernet/ansible
    ~/containernet/ansible$ sudo ansible-playbook -i "localhost," -c local install.yml
    ~$ cd ..
    ~/containernet$ sudo python setup.py install


Then, you can run containernet_wifi.py

.. code:: console

    ~/containernet$ sudo python examples/containernet_wifi.py


**Demo Video:**

- [https://www.youtube.com/watch?v=3iL9P9T0UdQ](https://www.youtube.com/watch?v=3iL9P9T0UdQ)
