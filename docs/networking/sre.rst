SRE (Site Reliability Engineer)
===============================

Notes
-----

Notes taken from `Google I/O 2011: Life in App Engine Production https://www.youtube.com/watch?v=rgQm1KEIIuc`_

#. Standalone computers/nodes usually have no problems. Problems are
   compounded when they are interconnected. Whethere in a data center or
   data centers connected to other data centers all over the world.
#. Traffic with peak and valleys coming from users. Systems needs to be
   designed to work in these cases.
#. *Leslie Lamport* is a computer scientist working with distributed
   systems. Creator of paxos distributed consensus algorithm used widely
   at Google.
#. App Engine is a cloud computing environment built inside Google's
   cloud computing environment.
   * It does not run in its own data center. It runs across nodes.
   * Cloud computing environment depends on a lot of services (storage,
     lock, computer, networking, etc.). Each of these are managed by
     other SREs.
   * Handshake between different layers of cloud computing with
     gurantees on certain reliability parameters. Then, each layer lives
     within the bounds of the parameters set in the layer below it.
   * Services is as reliable as the weakest (least reliable) service it
     depends on.
#. Google products and environment are designed for in-place updates. No
   disruption to users.
   * However, other upgrades such as network, power (generator), other
     hardware upgrades are intrusive. In this case, data is probably
     going to be re-routed to other data centers in the meantime.
   * Asynchronous replication happening from one datacenter to another.
#. Typical Google server serving storage has a storage process talking
   to a power management process that monitors power and battery (UPS)
   local to server.
   * Advantage of this communication is that when power's out, server
     does not accept anymore writes from network service.
   * UPS backup basically designed to allow cached writes to be
     committed to disk.
#. When error happens in server (in the case of power outage and no
   writes happening), design allows error to ripple back to user
   allowing user to retry request again. Thus, no lost state in user's
   end.
