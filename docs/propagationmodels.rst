**************************
Propagation Models
**************************

Mininet-WiFi supports the following propagation models:

- Friis Propagation Loss Model,
- Log-Distance Propagation Loss Model (default)
- Log-Normal Shadowing Propagation Loss Model
- International Telecommunication Union (ITU) Propagation Loss Model
- Two-Ray Ground Propagation Loss Model

How to add and configure a Propagation Model?
===================
You have to call the method net.setPropagationModel() like in `examples/propagationModel.py <https://github.com/intrig-unicamp/mininet-wifi/blob/master/examples/propagationModel.py>`_.

You may also want to consider some parameters, for example:

Friis Propagation Loss Model
===================

.. code:: console

    net.setPropagationModel(model="friis", sL = $int)

- sL = system loss

Log-Distance Propagation Loss Model
===================

.. code:: console

    net.setPropagationModel(model="logDistance", sL = $int, exp = $int)

- sL = system loss
- exp = exponent

Log-Normal Shadowing Propagation Loss Model
===================

.. code:: console

    net.setPropagationModel(model="logNormalShadowing", sL=$int, exp=$int, variance=$int)

- sL = system loss
- exp = exponent
- variance = gaussian variance
