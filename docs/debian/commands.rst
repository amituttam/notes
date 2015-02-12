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
