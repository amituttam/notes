SRE (Site Reliability Engineer)
===============================

Notes
-----

Notes taken from `Google I/O 2011: Life in App Engine Production <https://www.youtube.com/watch?v=rgQm1KEIIuc>`_

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

#. Monitoring processes lie to you sometimes. Idea is to *trust, but
   verify*.
   
   * Strong checksums at many layers of the stack are best.
   * It's like infinite monkey, eventually the monkey will write out
     shakespeare. When there are so many hardware, somehow something
     will end up deleting your data.

#. With asynchronous replication, data needs time to catch up to the
   slave data center. What if the link goes down? In this case, slave
   datacenter can't start serving data since it will be serving stale
   data.

   * Google has high replication datastore (synchronous writes).
   * Uses paxos algorithm do handle the synchronous writes.
   * If local data store is too slow, service will ignore its own local
     data store and write over network to remote data store.

#. How to design reliable services?

   * Your application/data should live in multiple data centers.

     * Data centers can't be in same location, power, network,
       continent, etc.
     * Be capable of handling multiple data center failures
       simultaneously.

   * Use synchronous writes to most of your datacenters.
   * Let your system/infrastructure decide what datacenters to read and
     write from. Don't wait for humans to react and make decisions.
   * Instantaneous traffic rerouting on demand.

#. Advantages of advance monitoring.

   * Idea is you should know you are having problems before customers
     start calling you.
   * For example, if data center is facing slightly longer latency for
     requests, the monitoring tools will alert you before your customers
     do. **Your customers are not your monitoring tools.**
   * Example:

     * CPU usage rockets up.
     * Explore requests, not that many to the home page but a few extra
       requests to /news.
     * Then, look at memcache and datastore rates. Memcache rates are
       about the same, but datastore rates are way up.
     * Thus, requests to /news requires a lot of work and data is not
       cached properly (not using memcached). Results are not cached.
     * Monitoring API running on app engine itself! High reliability
       platform.
