Firewall
========

.. contents:: :depth: 3

Notes from:

#. `Wikipedia - Firewall <http://en.wikipedia.org/wiki/Firewall_(computing)>`_

#. Firewall controls incoming and outgoing network based on applied rules.

#. Basically establishes a barrier between internal network and outside
   network.

#. Proxies can be firewalls by blocking certain connections from certain
   hosts or addresses.

#. Network Address Translation (NAT) has become an important part of
   firewalls. It hides addresses of hosts behind the firewall.

First Generation: Packet Filters
--------------------------------

#. First paper on firewall technology published was in 1988 by DEC.
   Talked about a *packet filter* firewall.

#. Act by inspecting packets. If it matches set of filtering rules, it
   silently drops packet or reject it with error message back to source.

#. The mechanism does not look at whether the packet is part of a
   connection stream, etc. Thus, it doesn't really maintain a state. It
   rejects based on looking at combination of source, destination
   address, protocol, port number, etc.

#. Pretty much works on the first three layers of OSI Model. It does
   peek into transport layer sometimes for source/destination port
   numbers.

#. Term originated in context of BSD operating systems. 

#. Examples are *iptables* for Linux and *PF* for BSD.

Second Generation: Stateful Filters
-----------------------------------

#. 1989-1990 from AT&T Bell Labs developed second gen firewall calling
   it circuit level gateway.

#. Operates up to Layer 4 (Transport Layer). Achieved by retaining
   enough packets in the buffer until enough information is availabe to
   make a judgement about its state.

#. Thus, it records all connections passing through it and determines if
   a packet is a part of current connection or new connection. Known as
   *stateful packet inspection*.

#. Can DoS by flooding firewall with thousands of fake connection
   packets.

Third Generation: application layer
-----------------------------------

#. Key benefit is that it can understand certain Application Layer
   protocols (FTP, HTTP, DNS).

#. Useful to detect if unwanted protocol is trying to use standard port
   from known applications (e.g. HTTP) to bypass firewall.

#. Can inspect if packet contains virus signatures.

#. Hooks into socket calls automatically.

#. Disadvantages are that it is quite slow and that rules can get
   complicated. It also can't possibly support of applications at
   application layer.
