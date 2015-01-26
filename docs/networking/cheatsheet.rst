Cheatsheet
==========

.. contents:: :depth: 3

Finding Duplicate IP Addresses on the Network
---------------------------------------------

1. Install **arp-scan**.
2. Run the following command:

.. code-block:: bash

    $ sudo arp-scan -I eth0 -l | grep 192.168.1.42
    $ OR
    $ sudo arp-scan -I eth0 -l | grep DUP
    192.168.1.92    00:c0:b7:55:f6:1f       AMERICAN POWER CONVERSION CORP (DUP: 2)
    192.168.1.152   00:1b:00:0f:55:0e       Neopost Technologies (DUP: 2)
    192.168.1.158   00:14:38:48:75:b7       Hewlett Packard (DUP: 2)

Capturing cupsd Traffic
-----------------------

First to get the list of open ports used by *cupsd*, use *netstat*:

.. code-block::  none

    $ sudo netstat -lntup | grep cupsd

    tcp        0      0 0.0.0.0:631        0.0.0.0:*  LISTEN      512/cupsd 
    tcp6       0      0 :::631             :::*       LISTEN      512/cupsd
    udp        0      0 0.0.0.0:631        0.0.0.0:*              512/cupsd

*tcpdump* can't be used so need to use *strace* here:

.. code-block:: none

    $ sudo strace -p 512 -f -e trace=network -s 10000 -o capture.out &
    $ sudo lpinfo -v

This will capture the HTTP messages (IPP) received by cupsd.
