```python
>>> f = open('/dev/spidev1.0', 'r')
>>> f.read()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IOError: [Errno 90] Message too long
```

Minimum SPI speed is 132096120 Hz / 255 = 518024 Hz.

Maximum speed is 132 MHz.

```bash
[root@rascal14:~]: cat /etc/init.d/rascal-gpio.sh 
for pin in 55 56 64 65 66 67 68 69 70 71 72 73 96 97 98 99 100 101 107
do
    echo $pin > /sys/class/gpio/export
    echo out > /sys/class/gpio/gpio$pin/direction
done
```

### Building LAPACK ###

Need to set ARCH to arm-angstrom-linux-gnueabi-ar and ranlib to arm-angstrom-linux-gnueabi-ranlib in make.inc.

```makefile
OPTS     = -shared -O2 -fPIC
NOOPT    = -O0 -fPIC
ARCH     = arm-angstrom-linux-gnueabi-ar
RANLIB   = arm-angstrom-linux-gnueabi-ranlib
```

```sh
tar xzvf lapack-3.4.1.tgz
cd lapack-3.4.1/SRC
make
gfortran  -shared -O2 -fPIC -c sgbbrd.f -o sgbbrd.o
gfortran  -shared -O2 -fPIC -c sgbcon.f -o sgbcon.o
gfortran  -shared -O2 -fPIC -c sgbequ.f -o sgbequ.o
gfortran  -shared -O2 -fPIC -c sgbrfs.f -o sgbrfs.o
gfortran  -shared -O2 -fPIC -c sgbsv.f -o sgbsv.o
gfortran  -shared -O2 -fPIC -c sgbsvx.f -o sgbsvx.o
gfortran  -shared -O2 -fPIC -c sgbtf2.f -o sgbtf2.o
gfortran  -shared -O2 -fPIC -c sgbtrf.f -o sgbtrf.o
```

### Building Scipy ###

```sh
tar xzvf scipy-0.11.0rc2.tar.gz
export BLAS=/home/root/libblas.so
export LAPACK=/home/root/liblapack.a
python setup.py build
...
error: Command "ar -cr build/temp.linux-armv5tejl-2.6/libdfftpack.a..." failed with exit status 1
```

Error was because Busybox's version of ar doesn't support -cr flags. Need to use arm-angstrom-linux-gnueabi-ar instead.

```sh
cd /usr/bin
mv ar busybox-ar
ln -s arm-angstrom-linux-gnueabi-ar ar
ls -l ar
lrwxrwxrwx    1 root     root           29 Sep  5 00:45 ar -> arm-angstrom-linux-gnueabi-ar
```

Then ran into an error with missing numpy/ndarraytypes.h file.

```sh
scipy/spatial/qhull/src/mem.h:23:32: error: numpy/ndarraytypes.h: No such file or directory
```

Using Numpy 1.4.1 on Rascal, but this file doesn't appear until 1.5.0. Downgraded Scipy to 0.8.0, and it's now building correctly. Also tested 0.10.1 and 0.9.0, but both needed missing header file.

### Problem building Scipy 0.8.0 ###

```sh
scipy/special/specfun/specfun.f: In function 'cva2':
scipy/special/specfun/specfun.f:1627: warning: 'iflag' may be used uninitialized in this function
scipy/special/specfun/specfun.f: In function 'cjylv':
scipy/special/specfun/specfun.f:1432: warning: 'IMAGPART_EXPR <cfj>' may be used uninitialized in this function
scipy/special/specfun/specfun.f:1432: warning: 'REALPART_EXPR <cfj>' may be used uninitialized in this function
scipy/special/specfun/specfun.f:1437: warning: 'IMAGPART_EXPR <cfy>' may be used uninitialized in this function
scipy/special/specfun/specfun.f:1437: warning: 'REALPART_EXPR <cfy>' may be used uninitialized in this function
gfortran: Internal error: Killed (program f951)
```

Found in dmesg:
```sh
sshd invoked oom-killer: gfp_mask=0x201da, order=0, oom_adj=0, oom_score_adj=0
[<c011e49c>] (unwind_backtrace+0x0/0xf0) from [<c015e570>] (dump_header+0x48/0x114)
[<c015e570>] (dump_header+0x48/0x114) from [<c015e76c>] (oom_kill_process+0x48/0x18c)
[<c015e76c>] (oom_kill_process+0x48/0x18c) from [<c015eaf4>] (out_of_memory+0x244/0x2c4)
[<c015eaf4>] (out_of_memory+0x244/0x2c4) from [<c01614d4>] (__alloc_pages_nodemask+0x45c/0x568)
[<c01614d4>] (__alloc_pages_nodemask+0x45c/0x568) from [<c0163510>] (__do_page_cache_readahead+0xa0/0x1e8)
[<c0163510>] (__do_page_cache_readahead+0xa0/0x1e8) from [<c0163680>] (ra_submit+0x28/0x30)
[<c0163680>] (ra_submit+0x28/0x30) from [<c015d9dc>] (filemap_fault+0x1b0/0x388)
[<c015d9dc>] (filemap_fault+0x1b0/0x388) from [<c016db38>] (__do_fault+0x50/0x3dc)
[<c016db38>] (__do_fault+0x50/0x3dc) from [<c016ee78>] (handle_mm_fault+0x2c4/0x3c8)
[<c016ee78>] (handle_mm_fault+0x2c4/0x3c8) from [<c011f3b8>] (do_page_fault+0xdc/0x1d0)
[<c011f3b8>] (do_page_fault+0xdc/0x1d0) from [<c0118258>] (do_PrefetchAbort+0x38/0x9c)
[<c0118258>] (do_PrefetchAbort+0x38/0x9c) from [<c0118d80>] (ret_from_exception+0x0/0x10)
Exception stack(0xc3a15fb0 to 0xc3a15ff8)
5fa0:                                     000837e4 000837fc 00000000 00000000
5fc0: 4031eb88 00000000 000837e0 000837fc 00000000 000837c4 4031b840 0000002c
5fe0: 00000000 bee86078 4021e984 4027cbb8 60000010 ffffffff
Mem-info:
Normal per-cpu:
CPU    0: hi:   18, btch:   3 usd:   3
active_anon:6854 inactive_anon:6987 isolated_anon:0
 active_file:22 inactive_file:125 isolated_file:0
 unevictable:0 dirty:0 writeback:0 unstable:0
 free:252 slab_reclaimable:194 slab_unreclaimable:366
 mapped:14 shmem:831 pagetables:134 bounce:0
Normal free:1008kB min:1016kB low:1268kB high:1524kB active_anon:27416kB inactive_anon:27948kB active_file:88kB inactive_file:500kB unevictable:0kB isolated(anon):0kB isolated(file):0kB present:65024kB mlocked:0kB dirty:0kB writeback:0kB mapped:56kB shmem:3324kB slab_reclaimable:776kB slab_unreclaimable:1464kB kernel_stack:360kB pagetables:536kB unstable:0kB bounce:0kB writeback_tmp:0kB pages_scanned:890 all_unreclaimable? yes
lowmem_reserve[]: 0 0
Normal: 8*4kB 2*8kB 0*16kB 0*32kB 1*64kB 1*128kB 1*256kB 1*512kB 0*1024kB 0*2048kB 0*4096kB = 1008kB
980 total pagecache pages
16384 pages of RAM
369 free pages
1087 reserved pages
560 slab pages
140 pages shared
0 pages swap cached
[ pid ]   uid  tgid total_vm      rss cpu oom_adj oom_score_adj name
[  410]     0   410      533       52   0     -17         -1000 udevd
[  584]     0   584      722       18   0       0             0 udhcpc
[  623]    43   623      645       42   0       0             0 dbus-daemon
[  633]     0   633     1080       68   0     -17         -1000 sshd
[  638]     0   638      486       26   0       0             0 cron
[  646]     0   646      721       15   0       0             0 syslogd
[  648]     0   648      721       18   0       0             0 klogd
[  654]    45   654      782       54   0       0             0 avahi-daemon
[  655]    45   655      782       46   0       0             0 avahi-daemon
[  669]     0   669      532       52   0     -17         -1000 udevd
[  670]     0   670      532       52   0     -17         -1000 udevd
[  684]     0   684      461       17   0       0             0 getty
[  691]     0   691     1103      133   0       0             0 sshd
[  695]     0   695      803       32   0       0             0 sh
[  713]     0   713     4157     2132   0       0             0 python
[  734]     0   734      721       16   0       0             0 sh
[  735]     0   735      721       16   0       0             0 sh
[  736]     0   736      467       20   0       0             0 gfortran
[  737]     0   737      721       16   0       0             0 tee
[  738]     0   738    12586    10245   0       0             0 f951
[  751]     0   751      486       27   0       0             0 cron
[  752]     0   752      721       23   0       0             0 sh
Out of memory: Kill process 738 (f951) score 639 or sacrifice child
Killed process 738 (f951) total-vm:50344kB, anon-rss:40956kB, file-rss:24kB
```