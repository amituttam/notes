Commands
========

Finding hardlink of file. For example ``zipinfo``:

.. code-block:: shell

    $ ls -l /usr/bin/zipinfo 
    -rwxr-xr-x 2 root root 158360 Apr 24 14:41 /usr/bin/zipinfo

You can see the number 2 indicating number of links (including this
one).

#. Use ``ls -i`` to find ``inode`` number of  file:

.. code-block:: shell

    $ ls -i /usr/bin/zipinfo
    1187726 /usr/bin/zipinfo

#. Use find to search for that specific ``inode``:

.. code-block:: shell

    $ find /usr/bin/ -inum 1187726
    /usr/bin/unzip
    /usr/bin/zipinfo

