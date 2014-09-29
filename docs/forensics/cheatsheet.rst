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

Cloning Partition Table
-----------------------

Use **sfdisk**, this is part of the **util-linux** package. In debian, it is
found in */usr/sbin/sfdisk*.

For GPT based disks, use `**gdisk** <http://unix.stackexchange.com/a/60393>`_.

1. Copy the partition table from the source disk:

.. code-block:: shell

    # sfdisk -d /dev/sda > mbr

2. Restore the partition table on destination disk:

.. code-block:: shell

    # sfdisk /dev/sdb < mbr
