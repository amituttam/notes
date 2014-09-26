Google I/O 2011: More 9s Please: Under The Covers of the High Replication Datastore
===================================================================================

Notes taken from `Google I/O 2011: More 9s Please: Under The Covers of the High Replication Datastore <https://www.youtube.com/watch?v=xO015C3R6dw>`_

#. Two types of datastore in App Engine:

   * Master/Slave

     * This is the old style. There is one master that handles all the
       reads/writes and asynchronous writes happen to the slave.
   
   * High Replication

     * This is the new default style. There is no master in this one as
       writes happen to all nodes synchronously. All act as a collective
       master.

#. Datastore Stack:

   * The actual datastore is the highest level. This is schema-less
     storage and has advance query engine.
   * This sits atop *megastore* which is defined by a strict schema and
     queried using standard SQL.
   * Megastore is powered by *Bigtable* which is a distributed key-value
     store.
   * Finally, the file system that powers all this is *GFSv2*, a
     distributed filesystem.

#. Writes to Datastore

   * In a Master/Slave, write happens to Datacenter A and gets
     asynchronously writted to Datacenter B at a later time.
   * In High Replication, write happens to a majority of the replicas
     synchronously. The other replica(s) that don't get the write
     synchronously gets an asynchronous write scheduled. Or can be
     on-demand replication when Read comes in to that datastore and it
     realizes that it doesn't have that data.
   * Writes to Master/Slave is faster (20ms) compared to High
     Replication Datastore (45ms).
   * Read latency is about the same but read error rate in High
     Replication is way less (0.001% vs 1%). Thus, resulting in 5m vs 9h
     downtime.

#. Planned Maintenance

   * Master/Slave
