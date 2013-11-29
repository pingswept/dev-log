Power consumption of Cubietruck out of the box measured at 2 W, or 0.03 A @ 121.6 V AC. Probably only accurate +/-1 W, or maybe more.

Load bootloaders and filesystem image onto microSD card,
based on http://docs.cubieboard.org/zh/tutorials/ct1/installation/install_lubuntu_desktop_server_to_sd_card

    card=/dev/sde                                             
    ➜  ~  sudo dd if=/dev/zero of=${card} bs=1024 seek=544 count=128
    128+0 records in
    128+0 records out
    131072 bytes (131 kB) copied, 0.023906 s, 5.5 MB/s
    ➜  ~  sudo dd if=u-boot-sunxi-with-spl-ct-20131102.bin of=$card bs=1024 seek=8
    254+1 records in
    254+1 records out
    260260 bytes (260 kB) copied, 0.0230326 s, 11.3 MB/s
    ➜  ~  sudo fdisk /dev/sde

In fdisk, delete existing partition, then add two new partitions.
    
    Command (m for help): d
    Selected partition 1
    
    Command (m for help): n
    Partition type:
       p   primary (0 primary, 0 extended, 4 free)
       e   extended
    Select (default p): p
    Partition number (1-4, default 1): 1
    First sector (2048-7626751, default 2048): 2048
    Last sector, +sectors or +size{K,M,G} (2048-7626751, default 7626751): 133119
    
    Command (m for help): n
    Partition type:
       p   primary (1 primary, 0 extended, 3 free)
       e   extended
    Select (default p): 
    Using default response p
    Partition number (1-4, default 2): 2
    First sector (133120-7626751, default 133120): 133120
    Last sector, +sectors or +size{K,M,G} (133120-7626751, default 7626751): 
    Using default value 7626751
    
    Command (m for help): p
    
    Disk /dev/sde: 3904 MB, 3904897024 bytes
    100 heads, 35 sectors/track, 2179 cylinders, total 7626752 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk identifier: 0x00000000
    
       Device Boot      Start         End      Blocks   Id  System
    /dev/sde1            2048      133119       65536   83  Linux
    /dev/sde2          133120     7626751     3746816   83  Linux

    Command (m for help): w
    The partition table has been altered!
    
    Calling ioctl() to re-read partition table.
    
    WARNING: Re-reading the partition table failed with error 16: Device or resource busy.
    The kernel still uses the old table. The new table will be used at
    the next reboot or after you run partprobe(8) or kpartx(8)
    Syncing disks.

Strangely, despite the warning message, the fdisk stuff above seems to have worked.

After more fiddling (new microSD card, following the rest of the tutorial), it boots.

Connected to serial console with USB FTDI adapter to UART0 with three wires.

    screen /dev/ttyUSB0 115200

Some info for reference.

    root@cubietruck:~# ps auxc
    USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
    root         1  0.1  0.0   2624  1584 ?        Ss   20:53   0:05 init
    root         2  0.0  0.0      0     0 ?        S    20:53   0:00 kthreadd
    root         3  0.1  0.0      0     0 ?        S    20:53   0:05 ksoftirqd/0
    root         5  0.0  0.0      0     0 ?        S    20:53   0:00 kworker/u:0
    root         6  0.0  0.0      0     0 ?        S    20:53   0:00 migration/0
    root         7  0.0  0.0      0     0 ?        S    20:53   0:00 migration/1
    root         8  0.0  0.0      0     0 ?        S    20:53   0:00 kworker/1:0
    root         9  0.0  0.0      0     0 ?        S    20:53   0:01 ksoftirqd/1
    root        10  0.0  0.0      0     0 ?        S<   20:53   0:00 cpuset
    root        11  0.0  0.0      0     0 ?        S<   20:53   0:00 khelper
    root        12  0.0  0.0      0     0 ?        S    20:53   0:00 kdevtmpfs
    root        13  0.0  0.0      0     0 ?        S<   20:53   0:00 netns
    root        14  0.0  0.0      0     0 ?        S    20:53   0:00 sync_supers
    root        15  0.0  0.0      0     0 ?        S    20:53   0:00 bdi-default
    root        16  0.0  0.0      0     0 ?        S<   20:53   0:00 kintegrityd
    root        17  0.0  0.0      0     0 ?        S<   20:53   0:00 kblockd
    root        18  0.0  0.0      0     0 ?        S    20:53   0:00 khubd
    root        19  0.0  0.0      0     0 ?        S<   20:53   0:00 cpufreq_uevent
    root        21  0.0  0.0      0     0 ?        S<   20:53   0:00 cfg80211
    root        22  0.0  0.0      0     0 ?        D    20:53   0:00 usb-hardware-sc
    root        23  0.0  0.0      0     0 ?        S<   20:53   0:00 kfantasy
    root        24  0.0  0.0      0     0 ?        S<   20:53   0:00 rpciod
    root        25  0.0  0.0      0     0 ?        S    20:53   0:00 kswapd0
    root        26  0.0  0.0      0     0 ?        SN   20:53   0:00 ksmd
    root        27  0.0  0.0      0     0 ?        S    20:53   0:00 fsnotify_mark
    root        28  0.0  0.0      0     0 ?        S<   20:53   0:00 nfsiod
    root        29  0.0  0.0      0     0 ?        S<   20:53   0:00 cifsiod
    root        30  0.0  0.0      0     0 ?        S<   20:53   0:00 crypto
    root        44  0.0  0.0      0     0 ?        S    20:53   0:00 kworker/1:1
    root        45  0.0  0.0      0     0 ?        S    20:53   0:00 nandd
    root        46  0.0  0.0      0     0 ?        S    20:53   0:00 nfmtd
    root        47  0.0  0.0      0     0 ?        S    20:53   0:00 scsi_eh_0
    root        49  0.0  0.0      0     0 ?        S    20:53   0:00 kworker/u:2
    root        56  0.0  0.0      0     0 ?        S<   20:53   0:00 kmpathd
    root        57  0.0  0.0      0     0 ?        S<   20:53   0:00 kmpath_handlerd
    root        58  0.0  0.0      0     0 ?        S    20:53   0:00 cfinteractive
    root        59  0.0  0.0      0     0 ?        S<   20:53   0:00 binder
    root        60  0.0  0.0      0     0 ?        S<   20:53   0:00 codec_resume
    root        61  0.0  0.0      0     0 ?        S    20:53   0:01 hdmi proc
    root        62  0.1  0.0      0     0 ?        S    20:53   0:07 mmcqd/0
    root        63  0.0  0.0      0     0 ?        S<   20:53   0:00 deferwq
    root        64  0.0  0.0      0     0 ?        S    20:53   0:00 jbd2/mmcblk0p2-
    root        65  0.0  0.0      0     0 ?        S<   20:53   0:00 ext4-dio-unwrit
    root       149  0.0  0.0   2068   716 ?        S    20:53   0:00 upstart-udev-br
    root       162  0.0  0.0   2264  1080 ?        Ss   20:53   0:00 udevd
    102        168  0.0  0.0   2524  1000 ?        Ss   20:53   0:00 dbus-daemon
    syslog     180  0.0  0.0  30284  1420 ?        Sl   20:53   0:00 rsyslogd
    root       246  0.0  0.0      0     0 ?        S<   20:53   0:00 mali-pmm-wq
    root       315  0.0  0.0   1904   448 ?        S    20:53   0:00 upstart-socket-
    root       362  0.0  0.1   4092  2040 ?        Ss   20:53   0:00 dhclient
    root       403  0.0  0.2  20136  5320 ?        Ssl  20:53   0:00 NetworkManager
    root       409  0.0  0.1  24216  2976 ?        Sl   20:53   0:00 polkitd
    root      1636  0.0  0.1   5004  2008 ?        Ss   20:57   0:00 sshd
    root      1856  0.0  0.0   1792   696 tty4     Ss+  20:58   0:00 getty
    root      1858  0.0  0.0   1792   696 tty5     Ss+  20:58   0:00 getty
    root      1861  0.0  0.0   3280  1484 ttyS0    Ss   20:58   0:00 login
    root      1870  0.0  0.0   1792   696 tty2     Ss+  20:58   0:00 getty
    root      1871  0.0  0.0   1792   696 tty3     Ss+  20:58   0:00 getty
    root      1874  0.0  0.0   1792   696 tty6     Ss+  20:58   0:00 getty
    root      1879  0.0  0.0   1844   760 ?        Ss   20:58   0:00 cron
    root      1917  0.0  0.0   2260   684 ?        S    20:58   0:00 udevd
    root      1919  0.0  0.0   2260   680 ?        S    20:58   0:00 udevd
    root      1922  0.0  0.1  28348  3264 ?        Sl   20:58   0:00 console-kit-dae
    root      1924  0.0  0.0   3280  1488 tty1     Ss   20:58   0:00 login
    root      2013  0.0  0.0   2564  1468 ttyS0    S    20:58   0:00 bash
    root      2014  0.0  0.0   2564  1460 tty1     S+   20:58   0:00 bash
    root      2083  0.2  0.0      0     0 ?        S    21:03   0:10 kworker/0:1
    root      2214  0.0  0.0      0     0 ?        S    21:52   0:00 kworker/0:2
    root      2217  0.0  0.0      0     0 ?        S    22:01   0:00 flush-179:0
    root      2219  0.0  0.0   2468   872 ttyS0    R+   22:01   0:00 ps

    root@cubietruck:~# ifconfig
    eth0      Link encap:Ethernet  HWaddr 9a:8e:9b:fd:4a:38  
              inet addr:192.168.1.112  Bcast:192.168.1.255  Mask:255.255.255.0
              inet6 addr: fe80::988e:9bff:fefd:4a38/64 Scope:Link
              UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
              RX packets:3504 errors:0 dropped:0 overruns:0 frame:0
              TX packets:1025 errors:0 dropped:0 overruns:0 carrier:0
              collisions:0 txqueuelen:1000 
              RX bytes:332826 (332.8 KB)  TX bytes:56620 (56.6 KB)
              Interrupt:117 Base address:0x8000 
    
    lo        Link encap:Local Loopback  
              inet addr:127.0.0.1  Mask:255.0.0.0
              inet6 addr: ::1/128 Scope:Host
              UP LOOPBACK RUNNING  MTU:16436  Metric:1
              RX packets:16 errors:0 dropped:0 overruns:0 frame:0
              TX packets:16 errors:0 dropped:0 overruns:0 carrier:0
              collisions:0 txqueuelen:0 
              RX bytes:1184 (1.1 KB)  TX bytes:1184 (1.1 KB)

    root@cubietruck:~# lsmod
    Module                  Size  Used by
    mali                  111175  0 
    ump                    52335  1 mali
    lcd                     3794  0 
    sunxi_gmac             30907  0 
    pwm_sunxi               9251  0 
