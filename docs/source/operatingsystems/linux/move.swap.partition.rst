.. _moving-swap-partition:

=======================
Moving Swap Partition on Ubuntu 24.04
=======================

Summary
=======
This guide provides step-by-step instructions to relocate a swap partition to the end of a disk on Ubuntu 24.04. The process involves disabling the current swap, adjusting partition tables, creating a new swap partition, and updating system configurations. Careful execution is required to avoid data loss or system instability.

Prerequisites
=============
- Backup critical data.
- Root or sudo access.
- Familiarity with terminal commands and disk partitioning tools (e.g., ``parted``, ``fdisk``).
- Ensure free unallocated space at the end of the disk.

Step-by-Step Instructions
=========================

1. Verify Current Swap Configuration
------------------------------------
Check active swap partitions:

.. code-block:: bash

   sudo swapon --show

2. Disable Swap
---------------
Disable all swap devices:

.. code-block:: bash

   sudo swapoff -a

3. Delete Old Swap Partition
----------------------------
Use a partitioning tool like ``parted`` or ``fdisk`` to delete the existing swap partition. Replace ``/dev/sdX`` with your disk identifier (e.g., ``/dev/sda``):

.. code-block:: bash

   sudo parted /dev/sdX
   (parted) print          # Identify the swap partition number
   (parted) rm <partition_number>
   (parted) quit

4. Create New Swap Partition at Disk End
----------------------------------------
Create a new partition in the unallocated space at the disk's end:

.. code-block:: bash

   sudo parted /dev/sdX
   (parted) mkpart primary linux-swap <start> <end>  # Use remaining space
   (parted) quit

5. Format and Enable New Swap
-----------------------------
Format the partition and activate it:

.. code-block:: bash

   sudo mkswap /dev/sdXN      # Replace N with the new partition number
   sudo swapon /dev/sdXN

6. Update ``/etc/fstab``
------------------------
Replace the old swap UUID with the new one. Retrieve the new UUID:

.. code-block:: bash

   sudo blkid /dev/sdXN

Edit ``/etc/fstab``:

.. code-block:: bash

   sudo nano /etc/fstab

Update the swap entry:

.. code-block:: text

   UUID=<new_uuid> none swap sw 0 0

7. Update systemd Configuration (Optional)
------------------------------------------
If using systemd, regenerate swap units:

.. code-block:: bash

   sudo systemctl daemon-reload

8. Verify and Reboot
--------------------
Confirm the swap is active:

.. code-block:: bash

   free -h

Reboot the system to ensure persistence.

References
==========
- `Ubuntu Swap Documentation <https://help.ubuntu.com/community/SwapFaq>`_
- `Parted Manual <https://www.gnu.org/software/parted/manual/parted.html>`_
- `systemd Swap Units <https://www.freedesktop.org/software/systemd/man/systemd.swap.html>`_

Metadata
========
:Tags: Ubuntu, System Administration, Partitioning, Swap, Linux
:Date Requested: 2024-06-20
:Deepseek Parameters: 
   - Model: DeepSeek-R1
   - Temperature: 0.8
   - Max Tokens: 4096
:Context: User requested instructions to move a swap partition to the end of the disk for Ubuntu 24.04, formatted as reStructuredText with specific sections and elements.
