Troubleshooting
===============

.. contents:: :depth: 2

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
