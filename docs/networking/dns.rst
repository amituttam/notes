DNS
===

.. contents:: :depth: 3

Overview
--------

Notes taken from `A DNS Primer <http://danielmiessler.com/study/dns/>`_

DNS is the Domain Name System used for obtaining IP Addresses from FQDN
(Fully Qualified Domain Names). An FQDN is an absolute name and provides
the exact location in the tree hierarchy of the domain name system.

But it is more than that, given a name, it finds resources associated
with that name. This is accomplished through a distributed database
system where requests for names are handed off to various tiers of
servers which are delinieated by the dot (.). It resolves from right to
left:

#. The root domain (dot)

#. The top level domain (TLD)

#. The second level domain

#. The subdomain

#. The host/resource name

When clients make requests, they make recursive queries (rather thand
iterative queries) which lets the DNS server to do the work of getting
the answer iteratively and thus returning only the final answer to the
client.

Typical Name Resolution
-----------------------

When a client makes a request, the following usually happens:

#. The DNS server is configured with an initial cached (hints) of known
   addresses of root name servers. This is updated periodically in an
   authoritative way.

#. Server receives requests from client and services it using its cache
   first (usually an answer is cached from previous lookups). If not, it
   performs the following steps for client.

#. Query is made to one of root servers to find server that is
   authoritative for top-level domain being requested.

#. Answer is received that points to nameserver of the top-level domain
   resource.

#. Server *walks the tree* from right to left, sending requests from
   nameserver to nameserver, until final step which returns IP of host
   in question.

#. IP Address of requested resource is given to client.

Protocol
--------
