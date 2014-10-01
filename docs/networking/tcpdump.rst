tcpdump
=======

Network packet capture tool in the CLI. `tcpdump` is extremely useful
for quick debugging of network issues. Use the `pcap-filter` manpage for
the filter syntax reference.

Capturing Traffic on Localhost
------------------------------

During development, there is usually a local webserver setup in
`http://localhost`. Custom apps/scripts are tested against this local
webserver to make them functionally correct. Thus, it is important to be
able to analyze traffic to and from the local webserver. Using the local
webserver for traffic analysis helps as there are no external traffic
that will confuse the analysis.

To capture `localhost` traffic:

.. code-block:: shell

    sudo tcpdump -A -v --number -i lo tcp port http

* `-A` is used to decode protocol in `ASCII`.
* `-v` is used for verbose mode. This allows us to see `tcp` communication details (flags, sequence numbers, etc).
* `--number` denomitate the packets
* `-i lo` use local loopback interface
* `tcp port http` the filter specifying protocol and port to use for capture.

Use `-l` for line buffering to see data while capturing it to a file.

.. code-block:: shell

    sudo tcpdump -l -A -v --number -i lo tcp port http | tee /tmp/capture

Capturing GMail Traffic
-----------------------

GMail goes over IMAP but not the standard IMAP port (143), it uses 993:

.. code-block:: shell

    sudo tcpdump -vvv -X --number -i wlan0 host 192.168.1.24 and tcp port 993

Use ``-vvv`` (three is max) to decode max level of the packets. Then use
*-X* to decode in Hex and ASCII.

Dropped Packets by the Kernel
-----------------------------

tcpdump uses a little buffer in the kernel to store captured packets. If
too many new packets arrive before the user process tcpdump can decode
them, the kernel drops them to make room for freshly arriving packets.

Use *-B* to increase the buffer. This is in units of KiB (1024 bytes).
