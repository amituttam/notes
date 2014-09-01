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

.. code-block:: html

    sudo tcpdump -A -v --number -i lo tcp port http

* `-A` is used to decode protocol in `ASCII`.
* `-v` is used for verbose mode. This allows us to see `tcp` communication
details (flags, sequence numbers, etc).
* `--number` denomitate the packets
* `-i lo` use local loopback interface
* `tcp port http` the filter specifying protocol and port to use for
capture.
