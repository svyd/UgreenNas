Boot from Micro SD Card.

Enable PCIe connector: see https://wiki.geekworm.com/X1001

Verify the NVMe drive is detected.  
Open a terminal and run `lsblk` to see a list of connected drives. The NVMe drive will likely appear as `/dev/nvme0n1`. 

By default the PCIe connector is not enabled. To enable it you should add the following option into /boot/firmware/config.txt and reboot:

```bash
sudo nano /boot/firmware/config.txt
```

Then add the following comment;
```
# Enable the PCIe External connector.
dtparam=pciex1
# This line is an alias for above (you can use either/or to enable the port).
dtparam=nvme
```
Press Ctrl-O, then enter, to write the change to the file.

Press Ctrl-X to exit nano (the editor).

```bash
sudo reboot
```

## Step 1: Prepare the Raspberry Pi and NVMe Drive

```bash
sudo apt update && sudo apt full-upgrade
sudo reboot
```

Verify the NVMe drive is detected. Open a terminal and run lsblk to see a list of connected drives. The NVMe drive will likely appear as `/dev/nvme0n1`. 

## Step 2: Copy Data from microSD to NVMe 

```bash
sudo dd if=/dev/mmcblk0 of=/dev/nvme0n1 bs=4M status=progress
```

_For my 512 Gb micro sd it takes ~2 hours_

## Step 3: Configure Boot Order and Reboot 

```bash
sudo raspi-config
```
Navigate to Advanced Options > Boot Order.  
Select NVMe/USB Boot (or a similar option that prioritizes NVMe).  
This tells the Pi to look for a bootable NVMe drive first.  
Select Finish and agree to reboot the system. 

You cloned a 512 GB SD card bit-for-bit onto a 1 TB NVMe, so the partition table and filesystem are still 512 GB.
The remaining ~455 GB is unallocated.
You must resize the partition and filesystem to use the full NVMe capacity.

So your NVMe disk now looks like a 512 GB disk written onto a 1 TB device.

```bash
nvme0n1     931.5G
├─nvme0n1p1 512M
└─nvme0n1p2 476.2G
```
Expand partition nvme0n1p2:

### Step 1: Grow the partition
```bash
sudo growpart /dev/nvme0n1 2
```
This extends partition 2 to the end of the disk.

Verify:
```bash
lsblk
```

You should now see nvme0n1p2 ≈ 931 GB.

### Step 2: Grow the filesystem
Most Raspberry Pi OS installations use ext4:

```bash
sudo resize2fs /dev/nvme0n1p2
```
This operation is online-safe (no unmount required).

Verify:
```bash
df -h /
```

Expected result:
```bash
/dev/nvme0n1p2  ~915G total
```
🎉 You now use the full NVMe.
