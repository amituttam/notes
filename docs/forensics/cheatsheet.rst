Cheatsheet
==========

Mounting E01 Images
-------------------

1. Install **ewf-tools** which contains **ewfmount**.
2. Mount the E01 image. This mounts it as a raw file.

.. code-block:: shell

    # ewfmount /srv/public/E01Capture/E01Capture.E01 /mnt/ewf

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
