# Runbook: Expanding Linux Guest Disk Partitions Live
This document outlines the steps to repartition and expand a filesystem inside a Linux guest OS after the underlying virtual disk (VMDK, EBS volume, etc.) has been expanded 
at the hypervisor or cloud provider level.


The utility growpart must be installed (packaged under cloud-utils on Ubuntu/Debian or cloud-utils-growpart on RHEL/CentOS).

## Step 1: Identify the Target Partition
Before making changes, verify the disk topology and find out which partition maps to the root (/) directory, as well as its filesystem type.

View the physical block devices and their layouts
```bash
sudo lsblk
```
# 2. Identify the filesystem type (Type) and mount point (Mounted on)
```bash
sudo df -Th
```
>Note: Pay attention to the disk name (e.g., sda) and the partition number hosting the root filesystem (e.g., sda2).

## Step 2: Expand the Partition Container
Use growpart to expand the partition table to the end of the newly allocated physical disk space. This process safely modifies the partition table live without affecting data.

Syntax: sudo growpart /dev/<disk_name> <partition_number>
```bash
sudo growpart /dev/sda 2
```
Note the space between the disk name and the partition number.

Verify the partition container grew successfully:

```bash
sudo lsblk
```
## Step 3: Resize the Filesystem
Now that the partition container is larger, the filesystem inside it must be stretched to occupy the new space. Run the command that matches the filesystem Type found in Step 1.

### For EXT4 Filesystems
```bash
sudo resize2fs /dev/sda2
```
### For XFS Filesystems
```bash
sudo xfs_growfs /
```
## Step 4: Verification
Confirm that the operating system recognizes the new storage capacity and that the utilization percentage has dropped.

```bash
sudo df -h
```
