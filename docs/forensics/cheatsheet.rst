Cheatsheet
==========

Mounting E01 Images
-------------------

1. Install **ewf-tools** which contains **ewfmount**.
2. Mount the E01 image. This mounts it as a raw file.

.. code-block:: shell

    # ewfmount /srv/public/E01Capture/E01Capture.E01 /mnt/ewf
    # or for multiple E01 files
    # ewfmount /srv/public/E01Capture/E01Capture.E* /mnt/ewf

3. Then use **mmls** from the **sleuthkit** package to analyze the raw
   image and find out where the partition offsets are. Note, in this
   case partition offset is (*512*2048=1048756*).

.. code-block:: shell

    # mmls /mnt/ewf/ewf1
    DOS Partition Table
    Offset Sector: 0
    Units are in 512-byte sectors

             Slot    Start        End          Length       Description
    00:  Meta    0000000000   0000000000   0000000001   Primary Table (#0)
    01:  -----   0000000000   0000002047   0000002048   Unallocated
    02:  00:00   0000002048   0007890943   0007888896   NTFS (0x07)
    03:  -----   0007890944   0007892991   0000002048   Unallocated

4. Mount the filesystem using regular *mount*:

.. code-block:: shell

    # mount -t ext4 -o ro,loop,offset=1048576 /mnt/ewf/ewf1 /mnt/usb
    # or for ntfs
    # mount -t ntfs-3g -o ro,nodev,noexec,show_sys_files,loop,offset=1048576 /mnt/ewf/ewf1 /mnt/usb

Setting HPA
-----------

Use *hdparm* with the *-N* option to find out the maximum number of
visible sectors:

.. code-block:: shell

    # hdparm -N /dev/sde

    /dev/sde:
     max sectors   = 64000/976773168, HPA is enabled

Then, to disable the HPA set it to the max visisble sectors:

.. code-block:: shell

    # hdparm --yes-i-know-what-i-am-doing -N p976773168 /dev/sde

    /dev/sde:
     setting max visible sectors to 976773168 (permanent)
      max sectors   = 976773168/976773168, HPA is disabled

Setting DCO
-----------

To identify DCO on disk:

.. code-block:: shell

    # hdparm --dco-identify /dev/sdb

To erase DCO on disk:

.. code-block:: shell

    # hdparm --yes-i-know-what-i-am-doing --dco-restore /dev/sdb

Cloning Partition Table
-----------------------

Use **sfdisk**, this is part of the **util-linux** package. In debian, it is
found in */usr/sbin/sfdisk*.

For GPT based disks, use `gdisk <http://unix.stackexchange.com/a/60393>`_.

1. Copy the partition table from the source disk:

.. code-block:: shell

    # sfdisk -d /dev/sda > mbr

2. Restore the partition table on destination disk:

.. code-block:: shell

    # sfdisk /dev/sdb < mbr

Inspecting Process Syscalls Using *sysdig*
------------------------------------------

Use **sysdig** to get detailed information about process system calls.
For example, to see what calls are being made by *iceweasel* do the
following:

.. code-block:: shell

    $ sudo sysdig proc.name=iceweasel
    10903 11:19:00.961549300 0 iceweasel (17398) > poll fds=5:e1 4:u1 8:p3 10:u1 22:p1 24:u1 3:f0 timeout=4294967295
    10908 11:19:00.961558641 0 iceweasel (17398) > switch next=0 pgft_maj=611 pgft_min=148114721 vm_size=2665740 vm_rss=1377504 vm_swap=0

The format of the output is quite similar to *tcpdump*. The output is as
follows:

.. code-block:: shell

    <evt.num> <evt.time> <evt.cpu> <proc.name> <thread.tid> <evt.dir> <evt.type> <evt.args>

    where:

    · evt.num is the incremental event number
    · evt.time is the event timestamp
    · evt.cpu is the CPU number where the event was captured
    · proc.name is the name of the process that generated the event
    · thread.tid id the TID that generated the event, which corresponds to the PID for single thread processes
    · evt.dir is the event direction, > for enter events and < for exit events
    · evt.type is the name of the event, e.g.  'open' or 'read'
    · evt.args is the list of event arguments.

You can also pass the *-w <capture>* to capture the trace to a file and
read it back using filters or *chisels* with *-r <capture>*.

**References**

#. `Sysdig + Logs: Advanced Log Analysis Made Easy <http://draios.com/sysdig-plus-logs/>`_
#. `Sysdig for ps, lsof, netstat + time travel <http://draios.com/ps-lsof-netstat-time-travel/>`_
#. `Hiding Linux Processes For Fun And Profit <http://draios.com/hiding-linux-processes-for-fun-and-profit/>`_

Check for problematic I/Os
--------------------------

Use **iostat** to see current read/write rates:

.. code-block:: shell

    $ sudo iostat -d 1
    Linux 3.16-2-amd64 (amit-debian)        10/02/2014      _x86_64_ (8 CPU)

    Device:            tps    kB_read/s    kB_wrtn/s    kB_read kB_wrtn
    sda               5.31        48.49        95.74    8472327 16726100

    Device:            tps    kB_read/s    kB_wrtn/s    kB_read kB_wrtn
    sda               0.00         0.00         0.00          0 0

*-d* is to show disk stats and *1* is to query every second.

To see I/Os and its respective processes with CPU usage, use **iotop**.

.. code-block:: shell

    $ sudo iotop
    Total DISK READ :       0.00 B/s | Total DISK WRITE :       7.64 K/s
    Actual DISK READ:       0.00 B/s | Actual DISK WRITE:      42.03 K/s
      TID  PRIO  USER     DISK READ  DISK WRITE  SWAPIN     IO> COMMAND
        168 be/3 root        0.00 B/s    7.64 K/s  0.00 %  2.80 % [jbd2/sda5-8]
        28565 be/4 root      0.00 B/s    0.00 B/s  0.00 %  0.27 % [kworker/1:5]
        26449 be/4 root      0.00 B/s    0.00 B/s  0.00 %  0.21 % [kworker/1:2]
        ...
