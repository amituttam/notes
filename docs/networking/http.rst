HTTP
====

Hypertext Transfer Protocol

Uses *base64* since HTTP is a protocol used to transfer text. To
transfer binary it must be encoded as text and sent out. This is what
*base64* is used for.

Basic format for requests/responses:

.. code-block:: html

    message = <start-line>
              *(<message-header>)
              CRLF
              [<message-body>]
     
    <start-line> = Request-Line | Status-Line 
    <message-header> = Field-Name ':' Field-Value

*Request* format:

.. code-block:: html

    Request-Line = Method SP URI SP HTTP-Version CRLF
    Method = "OPTIONS"
           | "HEAD"  
           | "GET"  
           | "POST"  
           | "PUT"  
           | "DELETE"  
           | "TRACE"

.. code-block:: html

    GET /articles/http-basics HTTP/1.1
    Host: www.articles.com
    Connection: keep-alive
    Cache-Control: no-cache
    Pragma: no-cache
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8

*Response* format:

.. code-block:: html

    Status-Line = HTTP-Version SP Status-Code SP Reason-Phrase CRLF

.. code-block:: html

    HTTP/1.1 200 OK
    Server: nginx/1.6.1
    Date: Tue, 02 Sep 2014 04:38:20 GMT
    Content-Type: text/html
    Last-Modified: Tue, 05 Aug 2014 11:18:35 GMT
    Transfer-Encoding: chunked
    Connection: keep-alive
    Content-Encoding: gzip

Error Codes
-----------

====  ==============
Code  Classification
====  ==============
1xx   Informational
2xx   Success
3xx   Redirection
4xx   Client Error
5xx   Server Error
===   ==============

Methods
-------

GET
^^^

Fetch a resource. Example in python:

.. code-block:: python

    def get():
    # Simple GET of index.html
    headers = { 'User-Agent': 'http_client/0.1',
                'Accept': '*/*',
                'Accept-Encoding': 'gzip, deflate' }
    http_conn = http.client.HTTPConnection("localhost")
    http_conn.set_debuglevel(1)
    http_conn.request("GET", "/", headers=headers)

    ## Response
    resp = http_conn.getresponse()
    print()
    print("Status:", resp.status, resp.reason)

    ## Cleanup
    http_conn.close()

Authentication
--------------

Basic Auth
^^^^^^^^^^

This is the simplest form of authentication since it doesn't require
cookies, session identifier or login pages. It uses standard HTTP
*Authorization* header to send login credentials. Thus, no handshakes
need to be done.

Typically used over *https* since encoding is done in *base64*
(passwords sent as plain text). Passwords can be easily decoded.

On *Server*, status code 401 is sent back and the following header is used:

.. code-block:: html

    WWW-Authenticate: Basic realm="Restricted"

On *Client*, the *Authorization* header is used with the following
format:

.. code-block:: html

    Authorization: Basic base64("username:password")

Example in python:

.. code-block:: python

    def get_auth():
    # GET with authorization of index.html
    authstring = base64.b64encode(("%s:%s" % ("amit","amit")).encode())
    authheader = "Basic %s" % (authstring.decode())
    print("Authorization: %s" % authheader)

    headers = { 'User-Agent': 'http_client/0.1',
                'Accept': '*/*',
                'Authorization': authheader,
                'Accept-Encoding': 'gzip, deflate' }
    http_conn = http.client.HTTPConnection("localhost")
    http_conn.set_debuglevel(1)
    http_conn.request("GET", "/", headers=headers)

    ## Response
    resp = http_conn.getresponse()
    print()
    print("Status:", resp.status, resp.reason)

    ## Cleanup
    http_conn.close()


Digest
^^^^^^

Basically uses MD5 of password and *nonce* value to prevent replay
attacks. Now, pretty much replaced by HMAC (keyed-hash message
authentication code).

A basic digest authentication session goes as follows:

#. HTTP client performs a request (GET, POST, PUT, etc)

#. HTTP server responds with a 401 error not authorized. In the
   response, a *WWW-Authenticate* header is sent that contains:

   * *Digest algorithm* - Usually *MD5*.
   * *realm* - The access realm. A string identifying the realm of the server.
   * *qop* - Stands for quality of protection (e.g. *auth*)
   * *nonce* - Server generated hash, issued only once per *401*
     response. Server should also have a timeout for the nonce values.

#. Client then receives the 401 status error and parses the header so it
   knows how to authenticate itself. It responds with the usual header
   and adds an *Authorization* header containing:

   * *Digest username*
   * *realm*
   * *nonce* - Sends the server generated value back.
   * *uri* - Sends the path to the resource it is requesting.
   * *algorithm* - The algorithm the client used to compute the hashes.
   * *qop*
   * *nc* - hexadecimal counter for number of requests.
   * *cnonce* - client generated nonce, always is generated per request.
   * *response* - Computed hash of ``md5(HA1:nonce:nc:cnonce:qop:HA2)``.

     * HA1 = ``md5(username:realm:password)``
     * HA2 = ``md5(<request method.:uri)``

   Notice how the client does not send the password in plain text.

#. Server computes hash and compares to client's hash and if it matches
   sends back *OK* with content. Note that *rspauth* sent back by server
   is a mutual authentication proving to client it knows its secret.

**Example HTTP Capture:**

.. code-block:: shell

    C:
    GET /files/ HTTP/1.1
    Host: localhost
    User-Agent: http_client/0.1
    Accept-Encoding: gzip, deflate
    Accept: */*

    S:
    HTTP/1.1 401 Unauthorized
    Server: nginx/1.6.1
    Date: Sat, 06 Sep 2014 02:09:24 GMT
    Content-Type: text/html
    Content-Length: 194
    Connection: keep-alive
    WWW-Authenticate: Digest algorithm="MD5", qop="auth", realm="Access
    Restricted", nonce="2a27b9b6540a6cd4"

    C:
    GET /files/ HTTP/1.1
    Host: localhost
    User-Agent: http_client/0.1
    Accept-Encoding: gzip, deflate
    Accept: */*
    Authorization: Digest username="amit", realm="Access Restricted",
    nonce="2a27b9b6540a6cd4", uri="/files/",
    response="421974c0c2805413b0d4187b9b143ecb", algorithm="MD5",
    qop="auth", nc=00000001, cnonce="e08190d5"

    S:
    .HTTP/1.1 200 OK
    Server: nginx/1.6.1
    Date: Sat, 06 Sep 2014 02:09:24 GMT
    Content-Type: text/html
    Transfer-Encoding: chunked
    Connection: keep-alive
    Authentication-Info: qop="auth", rspauth="33fea6914ddcc2a25b03aaef5d6b478b", cnonce="e08190d5", nc=00000001..
    Content-Encoding: gzip

**Example Python Code:**

.. code-block:: python

    def get_auth_digest():
        resp = get()

        # Get dictionary of headers
        headers = resp.getheader('WWW-Authenticate')
        h_list = [h.strip(' ') for h in headers.split(',')]
        #h_tuple = re.findall("(?P<name>.*?)=(?P<value>.*?)(?:,\s)", headers) 
        h_tuple = [tuple(h.split('=')) for h in h_list]
        f = lambda x: x.strip('"')
        h = {k:f(v) for k,v in h_tuple}
        print(h)

        # HA1 = md5(username:realm:password)
        ha1_str = "%s:%s:%s" % ("amit",h['realm'],"amit")
        ha1 = hashlib.md5(ha1_str.encode()).hexdigest()
        print("ha1:",ha1)

        # HA2 = md5(GET:uri) i.e. md5(GET:/files/)
        ha2_str = "%s:%s" % ('GET',path)
        ha2 = hashlib.md5(ha2_str.encode()).hexdigest()
        print("ha2:",ha2)

        # Generate cnonce
        cnonce = hashlib.sha1(str(random.random()).encode()).hexdigest()[:8]
        print("cnonce:",cnonce)

        # Generate response = md5(HA1:nonce:00000001:cnonce:qop:HA2)
        resp_str = "%s:%s:%s:%s:%s:%s" % (ha1,h['nonce'],"00000001",cnonce,h['qop'],ha2)
        resp_hash = hashlib.md5(resp_str.encode()).hexdigest()
        print("resp_hash:",resp_hash)

        # Do another get
        authheader = 'Digest username="%s", realm="%s", nonce="%s", ' \
                     'uri="%s", response="%s", algorithm="%s", qop="%s", nc=00000001, ' \
                     'cnonce="%s"' \
                     % ("amit", h['realm'], h['nonce'], path, resp_hash, h['Digest algorithm'], h['qop'], cnonce)
        print(authheader)
        headers = { 'User-Agent': 'http_client/0.1',
                    'Accept': '*/*',
                    'Accept-Encoding': 'gzip, deflate',
                    'Authorization': authheader
                  }
        get(headers)

Cookie Based
^^^^^^^^^^^^

Cookies are designed to maintain state. Thus, cookie based
authentication inherits this stateful principle. Cookie authentication
are the most common method used by web servers to know if the user is
still logged in or not. The browser keeps sending back the same cookie
to the server in every request.

Browsert uses **Set-Cookie** header to ask client to store the cookie.
The client uses **Cookie** header to send back the cookie to the server
so the server knows which client it is talking to.

Cookies are incompatible with *REST* style/architecture since *REST* is
stateless. According to *REST* style, cookies maintain site-wide state
while *REST* styles maintains application state. In *REST*, cookie
functionality can be achieved using anonymous authentication and
client-side state. *REST* also defines an alternative to cookies when
implementing shopping carts. According to *REST*:

*Likewise, the use of cookies to identify a user-specific "shopping
basket" within a server-side database could be more efficiently
implemented by defining the semantics of shopping items within the
hypermedia data formats, allowing the user agent to select and store
those items within their own client-side shopping basket, complete with
a URI to be used for check-out when the client is ready to purchase.*

Cookies have certain rules and attributes:

#. Name/value pair can't contain spaces or *;* *=*. Usually only ASCII
   characters. The *;* is uses as a delimiter.

#. The *Secure* attribute means this cookie is only used in encrypted
   communications.

#. The *HttpOnly* attribute means this cookie can only be used by
   http/https requests and not by JavaScript, etc. This prevents cross
   site scripting.

Other notes:

#. Not good practice to store username/password in cookies, even if it
   is hashed/salted, etc. Can be stolen and eventually cracked.

#. Cookie based authentication basically involves using the cookie the
   server sent to the client back to the server for every request.

Certificate Based
^^^^^^^^^^^^^^^^^

Idea is to separate those who verify password (the server will have a
copy or a hash of the password) and those who define the user identity.
Thus, certificate authority issues a private certificate to a user, and
guarantees that it can communicate using this key with the public key
issued to the other business party.

.. image:: images/02cert.png

The above image depicts certificate-based authentication. The client
asks the user to enter a password which unlocks the database holding the
private key. The client then uses this private key to sign a random data
and sends a certificate to the server. Thus, the password is never sent.

The `Red Hat Portal <https://access.redhat.com/documentation/en-US/Red_Hat_Certificate_System/8.0/html/Deployment_Guide/Introduction_to_Public_Key_Cryptography-Certificates_and_Authentication.html>`_ discusses this in great detail.

nginx `engineX`
---------------

Permissions
^^^^^^^^^^^

Make sure the permissions of the files in the directory are accessible
to the `other` group. Or change the permissions to the user that `nginx`
runs as (for debian it's `www-data`).

Setting up Basic Auth
^^^^^^^^^^^^^^^^^^^^^

1. Install **apache2-utils** to get **htpasswd**
2. Create an **.htpasswd** file in the web root. Make sure the
   permissions are *644*. Note that the password generated by *htpasswd*
   is an apache modified version of MD5.

.. code-block:: html

    sudo htpasswd -c /usr/share/nginx/html/.htpasswd amit

3. Update */etc/nginx/sites-available/default* in the location */* and
   reload *nginx*:

.. code-block:: html

    # Basic auth
    auth_basic "Restricted";
    auth_basic_user_file /etc/nginx/.htpasswd;

Setting up Digest Auth
^^^^^^^^^^^^^^^^^^^^^^

1. **apache2-utils** includes **htdigest** (similar to *htpasswd*) to
   generate digest key.
2. Create an **.htdigest** file in the web root. Make sure the
   permissions are *644*. Note that the *realm* here is *"Access
   Restricted"*.

.. code-block:: html

    sudo htdigest -c /usr/share/nginx/html/.htdigest "Access Restricted" amit

3. Need to build with *nginx-http-auth-digest* module from
   https://github.com/rains31/nginx-http-auth-digest. In order to do
   this, download *nginx* debian sources, copy *nginx-http-auth-digest*
   to *debian/modules*, and finally edit *debian/rules* to build
   *nginx-http-auth-digest* (look at *--add-module* config option).

4. Update */etc/nginx/sites-available/default* in the location */* and
   reload *nginx*:

.. code-block:: html

    # Digest auth
    auth_digest "Access Restricted";    # Realm
    auth_digest_user_file /usr/share/nginx/html/.htdigest;

Others
------

HTTPie - Command Line HTTP Client
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Very useful and feature rich command line http client written in Python
(http://github.com/jakubroztocil/httpie).

Useful for debugging HTTP requests. For example:

.. code-block:: html

    $ http get http://localhost
    HTTP/1.1 200 OK
    Connection: keep-alive
    Content-Encoding: gzip
    Content-Type: text/html
    Date: Mon, 01 Sep 2014 18:31:03 GMT
    Last-Modified: Tue, 05 Aug 2014 11:18:35 GMT
    Server: nginx/1.6.1
    Transfer-Encoding: chunked
    
    <!DOCTYPE html>
    <html>
    <head>
    <title>Welcome to nginx!</title>
    <style>
        body {
            width: 35em;
            margin: 0 auto;
            font-family: Tahoma, Verdana, Arial, sans-serif;
        }
    </style>
    </head>
    <body>
    <h1>Welcome to nginx!</h1>
    <p>If you see this page, the nginx web server is successfully installed and
    working. Further configuration is required.</p>
    
    <p>For online documentation and support please refer to
    <a href="http://nginx.org/">nginx.org</a>.<br/>
    Commercial support is available at
    <a href="http://nginx.com/">nginx.com</a>.</p>
    
    <p><em>Thank you for using nginx.</em></p>
    </body>
    </html>
