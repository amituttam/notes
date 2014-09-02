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

systemd
-------

1. Create */var/log/journal* where *systemd* can store persistent journals.
2. The directory should be owned by *systemd-journal*.
3. Fix the permissions for this group so that any user that is a member
   of *systemd-journal* should be able to access it.

.. code-block:: shell
    $ sudo chmod g+rwx /var/log/journal

4. Add the user to the *systemd-journal* group:

.. code-block:: shell
    $ sudo usermod -a -G systemd-journal logicube

5. Reboot the system using *systemct reboot*.

Enlightenment
-------------
