Setup
=====

Install
-------

The latest jessie install as of Sept 02, 2014 is
*debian-jessie-DI-b1-amd64-netinst.iso*. This already comes by default
with *systemd* as the init.

During the install, select all defaults and just select *SSH Server* and
*Standard System Utilities* in tasksel.

Basic Setup
-----------

1. Enable *force_color_prompt* in *.bashrc*.
2. Update apt lists and upgrade all packages.
3. Login using *su* and install *sudo*.
4. Add the following lines using *visudo*:

.. code-block:: shell

    # User privilege specification
    logicube        ALL=(ALL:ALL) ALL

5. Install *avahi-daemon* so we don't have to remember IP address. We
   don't have to use no recommends here since the list of packages it
   recommends is only one.

.. code-block:: shell

    $ sudo aptitude install avahi-daemon

6. Add *UseDNS no* to */etc/ssh/sshd_config* and restart *ssh* service.

systemd
-------

1. Create */var/log/journal* where *systemd* can store persistent journals.
2. The directory should be owned by *systemd-journal*.

.. code-block:: shell

    $ sudo chgrp systemd-journal /var/log/journal

3. Fix the permissions for this group so that any user that is a member
   of *systemd-journal* should be able to access it.

.. code-block:: shell

    $ sudo chmod g+rwx /var/log/journal

4. Add the user to the *systemd-journal* group:

.. code-block:: shell

    $ sudo usermod -a -G systemd-journal logicube

5. Reboot the system using *systemctl reboot*.

Enlightenment
-------------

1. Install basic *X*, use *-R* to not install recommended packages:

.. code-block:: shell

    $ sudo aptitude install -R xserver-xorg-core xinit xinput xserver-xorg-video-intel xserver-xorg-input-evdev x11-utils

2. Install a small display manager *CDM*
   https://wiki.archlinux.org/index.php/CDM.

   #. Install *dialog* package as a dependency. 
   #. Install *git* (--without-recommends) to get the latest CDM from https://github.com/ghost1227/cdm.
   #. Clone cdm to */tmp*.
   #. Run *sudo ./install.sh*.
   #. ``sudo cp /usr/share/doc/cdm/profile.sh /etc/profile.d/zzz-cdm.sh``
   #. ``chmod +x /etc/profile.d/zzz-cdm.sh``

3. Install *e17*:

.. code-block:: shell

    $ sudo aptitude install -R e17 fonts-droid librsvg2-common
