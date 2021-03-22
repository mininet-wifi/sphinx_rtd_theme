************
Download/Get Started With Mininet-WiFi
************

Option 1: Mininet-WiFi VM Installation (easy, recommended)
===================
Follow these steps for a VM install:

#. :ref:`Download the Mininet-WiFi VM image<https://intrig.dca.fee.unicamp.br:8840/owncloud/index.php/s/UfEWT4banmdQuH3>`
#. Download and install a virtualization system. We recommend VirtualBox (free, GPL) because it is free and works on OS X, Windows, and Linux (though it’s slightly slower than VMware in our tests.) You can also use Qemu for any platform, VMware Workstation for Windows or Linux, VMware Fusion for Mac, or KVM (free, GPL) for Linux.
#. Sign up for the :ref:`mininet-wifi-discuss<https://groups.google.com/forum/#!forum/mininet-wifi-discuss>` mailing list. This is the source for Mininet-WiFi support and discussion with the friendly Mininet-WiFi community. ;-)

Option 2: Native Installation from Source
===================

This option works well for local VM, remote EC2, and native installation. It assumes the starting point of a fresh Ubuntu, Debian (or, experimentally, Fedora) installation.

We strongly recommend more recent Ubuntu releases, because they support newer versions of Open vSwitch. (Fedora also supports recent OVS releases)

To install natively from source, first you need to get the source code:

::
    git clone git://github.com/intrig-unicamp/mininet-wifi


Note that the above git command will check out the latest and greatest Mininet (which we recommend!)

::
    cd mininet-wifi


Once you have the source tree, the command to install Mininet-WiFi is:

::
    sudo util/install.sh -Wlnfv


**install.sh options:**

* -W: wireless dependencies
* -l: wmediumd
* -n: mininet-wifi dependencies
* -f: OpenFlow
* -v: OpenvSwitch

**optional:**
-------------
* -6: wpan tools