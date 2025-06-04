## Re-install date: Jun 04, 2025

### Storage:
```
pi@prometheus:~ $ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            318M     0  318M   0% /dev
tmpfs            91M  2.5M   89M   3% /run
/dev/mmcblk0p2   57G  2.1G   52G   4% /
tmpfs           454M     0  454M   0% /dev/shm
tmpfs           5.0M   12K  5.0M   1% /run/lock
/dev/mmcblk0p1  510M   67M  444M  13% /boot/firmware
tmpfs            91M     0   91M   0% /run/user/1000
```
### Temperature:
```
pi@prometheus:~ $ vcgencmd measure_temp
temp=56.4'C
```
