IP
==

.. contents:: :depth: 3

Path MTU Discovery (PMUTD)
--------------------------

#. Determines the MTU (Maximum Transmission Unit) size on the network
   path between two IP hosts. (`RFC 1191 - Path MTU Discovery
   <https://tools.ietf.org/html/rfc1191>`_).

#. The goal is to avoid IP fragmentation.

#. In IPv6, this function has been explicitly delegated to the end
   points of a communications session. (`RFC 1981 - Path MTU Discovery
   for IP version 6 <http://tools.ietf.org/html/rfc1981>`_).

#. For IPv4 packets, Path MTU Discovery works by setting the Don't
   Fragment (DF) option bit in the IP headers of outgoing packets. Then,
   any device along the path whose MTU is smaller than the packet will
   drop it, and send back an Internet Control Message Protocol (ICMP)
   Fragmentation Needed (Type 3, Code 4) message containing its MTU,
   allowing the source host to reduce its Path MTU appropriately. The
   process is repeated until the MTU is small enough to traverse the
   entire path without fragmentation.

#. IPv6 routers do not support fragmentation or the Don't Fragment
   option. For IPv6, Path MTU Discovery works by initially assuming the
   path MTU is the same as the MTU on the link layer interface through
   which the traffic is being sent.

   * Then, similar to IPv4, any device along the path whose MTU is
     smaller than the packet will drop the packet and send back an
     ICMPv6 Packet Too Big (Type 2) message containing its MTU, allowing
     the source host to reduce its Path MTU appropriately. The process
     is repeated until the MTU is small enough to traverse the entire
     path without fragmentation.

#. Many network security devices block all ICMP messages for perceived
   security benefits,[6] including the errors that are necessary for the
   proper operation of PMTUD. This can result in connections that
   complete the TCP three-way handshake correctly, but then hang when
   data is transferred. This state is referred to as a black hole
   connection.

#. A robust method for PMTUD that relies on TCP or another protocol to
   probe the path with progressively larger packets has been
   standardized in `RFC 4821 <http://tools.ietf.org/html/rfc4821>`_.
