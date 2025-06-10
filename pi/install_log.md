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

Died on Jun 10, 2025 - for some reason I cant to ssh
pi@prometheus:~ $ date
Tue 10 Jun 14:35:40 EEST 2025
pi@prometheus:~ $ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            318M     0  318M   0% /dev
tmpfs            91M  3.1M   88M   4% /run
/dev/mmcblk0p2   57G  3.4G   51G   7% /
tmpfs           454M     0  454M   0% /dev/shm
tmpfs           5.0M   12K  5.0M   1% /run/lock
/dev/mmcblk0p1  510M   57M  454M  12% /boot/firmware
tmpfs            91M     0   91M   0% /run/user/1000

```
### Temperature:
```
pi@prometheus:~ $ vcgencmd measure_temp
temp=56.4'C

pi@prometheus:~ $ date
Thu  5 Jun 10:17:46 EEST 2025
pi@prometheus:~ $ vcgencmd measure_temp
temp=64.5'C

pi@prometheus:~ $ date
Fri  6 Jun 15:50:13 EEST 2025
pi@prometheus:~ $ vcgencmd measure_temp
temp=58.0'C
Died on Jun 10, 2025 - for some reason I cant to ssh

Install date: Jun 10, 2025
pi@prometheus:~ $ date
Tue 10 Jun 14:35:40 EEST 2025
pi@prometheus:~ $ vcgencmd measure_temp
temp=54.2'C
```
