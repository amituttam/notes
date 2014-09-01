HTTP
====

Hypertext Transfer Protocol

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
   permissions are *644*.

.. code-block:: html

    sudo htpasswd -c /usr/share/nginx/html/.htpasswd amit

3. Update */etc/nginx/sites-available/default* in the location */* and
   reload *nginx*:

.. code-block:: html

    # Basic auth
    auth_basic "Restricted";
    auth_basic_user_file /etc/nginx/.htpasswd;

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
