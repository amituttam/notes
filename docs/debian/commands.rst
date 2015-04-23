Commands
========

.. contents:: :depth: 3

Finding hardlink of file
------------------------

For example ``zipinfo``:

.. code-block:: bash

    $ ls -l /usr/bin/zipinfo 
    -rwxr-xr-x 2 root root 158360 Apr 24 14:41 /usr/bin/zipinfo

You can see the number 2 indicating number of links (including this
one).

#. Use ``ls -i`` to find ``inode`` number of  file:

.. code-block:: bash

    $ ls -i /usr/bin/zipinfo
    1187726 /usr/bin/zipinfo

#. Use find to search for that specific ``inode``:

.. code-block:: bash

    $ find /usr/bin/ -inum 1187726
    /usr/bin/unzip
    /usr/bin/zipinfo

When DKMS Build Fails Due to Missing Source
-------------------------------------------

The kernel headers are enought to build a module. However, if building
the DKMS fails and kernel headers package are installed, it usually
means there is no *build* symlink under */lib/modules/`uname -r`*.

Do the following to create the symlink:

.. code-block:: bash

    $ sudo ln -s /usr/src/linux-headers-$(uname -r)/ /lib/modules/$(uname -r)/build

No configure script, only configure.ac
--------------------------------------

Need to run the following set of commands to build a working configure script from a configure.ac:

.. code-block:: bash

    $ libtoolize --force
    $ aclocal
    $ autoheader
    $ automake --force-missing --add-missing
    $ autoconf
    $ ./configure --prefix=/usr

Transferring files over netcat
------------------------------

Sometimes this is useful when you need to transfer some files from a
unit that has *busybox* or booted up to an *initramfs* shell.

First, on PC that you would like the file to be transferred to:

.. code-block:: bash

    $ nc -l -p 6666 > dmesg.out

You can check if port *6666* is open by running:

.. code-block:: bash

    $ lsof -i :6666
    COMMAND PID USER   FD   TYPE   DEVICE SIZE/OFF NODE NAME
    nc      953 amit    3u  IPv4 54834351      0t0  TCP \*:6666 (LISTEN)

Then on busybox shell:

.. code-block:: bash

    (initramfs) ip link set dev eth0 up
    (initramfs) ip addr add 192.168.1.123/24 dev eth0
    (initramfs) dmesg > /tmp/dmesg
    (initramfs) nc 192.168.1.175:6666 < /tmp/dmesg
