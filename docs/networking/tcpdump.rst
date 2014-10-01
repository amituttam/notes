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

Get Time Delta Between Request/Response
---------------------------------------

Pass the *-ttt* flag to get the time delta between current line and
previous line.

.. code-block:: shell

    $ sudo tcpdump -nS -ttt port http and host snapshot.debian.org


    tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
    listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes




    00:00:00.000000 IP 192.168.1.170.34233 > 193.62.202.30.80: Flags [S], seq 1140376233, win 29200, options [mss 1460,sackOK,TS val 22265623 ecr 0,nop,wscale 7], length 0
    00:00:00.228373 IP 193.62.202.30.80 > 192.168.1.170.34233: Flags [S.], seq 1460190713, ack 1140376234, win 5792, options [mss 1350,sackOK,TS val 74072844 ecr 22265623,nop,wscale 7], length 0
    00:00:00.000040 IP 192.168.1.170.34233 > 193.62.202.30.80: Flags [.], ack 1460190714, win 229, options [nop,nop,TS val 22265680 ecr 74072844], length 0
    00:00:00.000119 IP 192.168.1.170.34233 > 193.62.202.30.80: Flags [P.], seq 1140376234:1140376399, ack 1460190714, win 229, options [nop,nop,TS val 22265680 ecr 74072844], length 165
    00:00:00.222658 IP 193.62.202.30.80 > 192.168.1.170.34233: Flags [.], ack 1140376399, win 54, options [nop,nop,TS val 74072902 ecr 22265680], length 0
    00:00:00.001001 IP 193.62.202.30.80 > 192.168.1.170.34233: Flags [P.], seq 1460190714:1460191405, ack 1140376399, win 54, options [nop,nop,TS val 74072902 ecr 22265680], length 691
    00:00:00.000032 IP 192.168.1.170.34233 > 193.62.202.30.80: Flags [.], ack 1460191405, win 239, options [nop,nop,TS val 22265736 ecr 74072902], length 0
    00:00:00.008210 IP 192.168.1.170.34233 > 193.62.202.30.80: Flags [F.], seq 1140376399, ack 1460191405, win 239, options [nop,nop,TS val 22265738 ecr 74072902], length 0
    00:00:00.183523 IP 193.62.202.30.80 > 192.168.1.170.34233: Flags [F.], seq 1460191405, ack 1140376400, win 54, options [nop,nop,TS val 74072960 ecr 22265738], length 0
    00:00:00.000060 IP 192.168.1.170.34233 > 193.62.202.30.80: Flags [.], ack 1460191406, win 239, options [nop,nop,TS val 22265784 ecr 74072960], length 0
