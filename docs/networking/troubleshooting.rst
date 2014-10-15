Troubleshooting
===============

.. contents:: :depth: 2

Identifying and Solving Performance Issues
------------------------------------------

Steps should be followed in order:

#. *Understand the problem*

   * Half the problem is solved when problem is understood clearly.

#. *Monitor and collect data*

   * Monitor the system and collect as much data as possible.

#. *Eliminate and narrow down issues*

   * Come up with a list of potential issues and possibly narrow them
     down eliminating any non issues.

#. *Make one change at a time*

   * Make one change and re-test. Don't make multiple changes at one
     time.

Common Server Problems
----------------------

#. A server is used as a proxy in order to perform cross domain
   requests. If the server returns *502* error it means: *The server,
   while acting as a gateway or proxy, received an invalid response from
   the upstream server it accessed in attempting to fulfill the request.*

#. Sometimes a server is not accessible why is that? Answer in terms of
   DNS.

   The DNS resolver's cache is controlled by the time-to-live (TTL)
   value that you set for your records and is specified in seconds. For
   example, if you set a TTL value of 86400 (the number of seconds in 24
   hours), the DNS resolvers are instructed to cache the records for 24
   hours. Some DNS resolvers ignore the TTL value or use their own
   values that can delay the full propagation of records.

   If you are planning for a change to services that requires a narrow
   window, you might want to change the TTL in advance of your change to
   a shorter TTL value. This change can help reduce the caching window
   and ensure a quicker change to your new record settings. After the
   change, you can change the value back to its previous TTL value to
   reduce load on the DNS resolvers.

Common AJAX Problems
--------------------

#. Although, almost all browsers support JavaScript today, there could
   be some users accessing it with JavaScript disabled.

   * Should design to degrade gracefully when it detects JavaScript is
     disabled.

   * Also build the website to work without JavaScript. Then, JavaScript
     should be used as an enhancement. Most users will see the page with
     JavaScript anyways.

#. Sometimes user doesn't know a request has completed. Thus, display a
   message or status indicating request has been completed.

#. AJAX requests can't access third-party web services.

   * The *XMLHttpRequest* object, which is at the root of all Ajax
     requests, is restricted to making requests on the same domain as
     the page making the request.

   * Solution is to use your server as the proxy to access API of third
     party web service.

   * Can also use jQuery to perform cross-domain requests (*$.ajax()*
     API).

   * Can use *<script>* tags that load JavaScript files from other
     domains. *JSONP (JSON Padding)* uses this technique to dynamically
     create *<script>* tag with necessary URL.

#. Using back button on sites with AJAX can sometimes revert the page
   back to its initial state.

   * Solution to this is to use internal page anchors. Basically, save
     the current URL and use that.

Common App Engine Problems
--------------------------

#. If you have timeouts in your application, you maybe updating a single
   entity group in your datastore too rapidly (about 5 times/sec).
   Datastore will be in contention. Design issue of BigTable.

   * Can be a problem if you have a entity that is a counter that you
     want to update faster than 5 times/sec.

   * Way to reduce problem is taking advantage of extremely cheap and
     fast reads from datastore. Thus, use *sharding* to split counters
     into *N* counters.

     * When you want to increment the counter, pick a shard at random
       and increment it.

     * When you want to read the total count, read all shards and sum up
       all counters.

     * The more shards, the higher throughput on increments to counters.

   * Another way is to make updates to memcache and periodically
     flushing to datastore.

   * Timeouts can happen if your data is in a tablet that is currently
     being moved around between servers for load balancing when you are
     trying to access it.

   * Timeouts can happen if you are writing large data and thus tablets
     are being split while you are writing. Use exponential backoff
     strategy before retrying since it takes sometimes couple hunder
     millisecs to a sec or two for tablets to be available.

#. To handle errors in datastore gracefully, make transactions
   *idempotent*. Thus, if you repeat the transaction, the end result
   will be the same.

#. If daily budget is exceeded in app engine, application could serve
   errors.

#. When upgrading to a new application, don't move all your users right
   away. Use traffic splitting to gradually move new requests to new
   version of your application. Cached objects may no longer be cached
   thus an increase in read latency.

   * IP Address splitting works by hashing IP Address to value of 0-999
     and splitting based on that.

   * Cookies give finer split control and more detailed statistics.
     Uses *GOOGAPPUID* cookie and range of 0-999.

   * Some issues with traffic splitting is cached documents. Use
     *Cache-Control* and *Expires* header to tell proxies that resource
     is dynamic.

#. To avoid thundering herd problem (where retries get compounded
   because first retry fails and all requests keep trying), use backoff
   on retry.
