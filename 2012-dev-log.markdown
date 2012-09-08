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

f951 is the Fortran compiler (the parser/assembly-generator front-end) that is part of Gfortran, which is part of GCC.

Tried commenting out config.add_subpackage line for *special* subpackage in scipy/setup.py. Seems to have at least made compiler work on something else.

```sh
scipy/interpolate/src/interpolate.h: In function 'void loginterp(T*, T*, int, T*, T*, int)':
scipy/interpolate/src/interpolate.h:56: error: 'lower_bound' is not a member of 'std'
scipy/interpolate/src/interpolate.h: In function 'int block_average_above(T*, T*, int, T*, T*, int)':
scipy/interpolate/src/interpolate.h:102: error: 'lower_bound' is not a member of 'std'
scipy/interpolate/src/interpolate.h: In function 'int window_average(T*, T*, int, T*, T*, int, T)':
scipy/interpolate/src/interpolate.h:145: error: 'lower_bound' is not a member of 'std'
scipy/interpolate/src/interpolate.h:153: error: 'lower_bound' is not a member of 'std'
...
In file included from scipy/interpolate/src/_interpolate.cpp:4:
scipy/interpolate/src/interpolate.h:3:20: error: iostream: No such file or directory
scipy/interpolate/src/interpolate.h:4:21: error: algorithm: No such file or directory
In file included from scipy/interpolate/src/_interpolate.cpp:4:
scipy/interpolate/src/interpolate.h: In function 'void linear(T*, T*, int, T*, T*, int)':
scipy/interpolate/src/interpolate.h:20: error: 'lower_bound' is not a member of 'std'
scipy/interpolate/src/interpolate.h: In function 'void loginterp(T*, T*, int, T*, T*, int)':
scipy/interpolate/src/interpolate.h:56: error: 'lower_bound' is not a member of 'std'
scipy/interpolate/src/interpolate.h: In function 'int block_average_above(T*, T*, int, T*, T*, int)':
scipy/interpolate/src/interpolate.h:102: error: 'lower_bound' is not a member of 'std'
scipy/interpolate/src/interpolate.h: In function 'int window_average(T*, T*, int, T*, T*, int, T)':
scipy/interpolate/src/interpolate.h:145: error: 'lower_bound' is not a member of 'std'
scipy/interpolate/src/interpolate.h:153: error: 'lower_bound' is not a member of 'std'
```

Problem appears to be missing iostream and algorithm headers. See http://members.gamedev.net/sicrane/articles/iostream.html

Hell, let's just comment out the interpolate subpackage too.

### At least sort of built Scipy 0.8.0 natively on the Rascal! ###

Had to skip *interpolate*, *sparse* and *special* packages by commenting out lines in scipy-0.8.0/scipy/setup.py.

Took around 10 hours in all.

Once the problem subpackages were commented out, the procedure was pretty simple.
```sh
tar xzvf scipy-0.8.0.tar.gz
cd scipy-0.8.0
export BLAS=/home/root/libblas.so
export LAPACK=/home/root/liblapack.a
python setup.py build
python setup.py install
```
Of course, I still don't know how much of it actually works, but at least it starts up OK.

```python
Python 2.6.6 (r266:84292, Jun 18 2012, 19:18:49) 
[GCC 4.3.3] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import scipy as sp
>>> sp.pi
3.1415926535897931
```
Problems with various subpackages:
* interpolate: scipy/interpolate/src/interpolate.h uses iostream and algorithm header files, which can't be found
* sparse: scipy/sparse/sparsetools/csr.h uses lots of C++ headers that aren't found
* special: scipy/special/specfun/specfun.f takes too much memory to compile

Maybe the C++ headers are in here?
```sh
opkg install libstdc++-dev
```
After the subpackages are uncommented, this fixes *interpolate* and maybe *sparse*, but *sparse* then takes too much memory, like *special*.

Error with sparse subpackage
```sh
building 'scipy.sparse.sparsetools._csr' extension
compiling C++ sources
C compiler: arm-angstrom-linux-gnueabi-g++ -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -fexpensive-optimizations -frename-registers -fomit-frame-pointer -O2 -ggdb2 -DNDEBUG -g -O3 -Wall -fPIC

compile options: '-I/usr/lib/python2.6/site-packages/numpy/core/include -I/usr/include/python2.6 -c'
arm-angstrom-linux-gnueabi-g++: scipy/sparse/sparsetools/csr_wrap.cxx
arm-angstrom-linux-gnueabi-g++: Internal error: Killed (program cc1plus)
Please submit a full bug report.
See <http://gcc.gnu.org/bugs.html> for instructions.
arm-angstrom-linux-gnueabi-g++: Internal error: Killed (program cc1plus)
Please submit a full bug report.
See <http://gcc.gnu.org/bugs.html> for instructions.
error: Command "arm-angstrom-linux-gnueabi-g++ -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -fexpensive-optimizations -frename-registers -fomit-frame-pointer -O2 -ggdb2 -DNDEBUG -g -O3 -Wall -fPIC -I/usr/lib/python2.6/site-packages/numpy/core/include -I/usr/include/python2.6 -c scipy/sparse/sparsetools/csr_wrap.cxx -o build/temp.linux-armv5tejl-2.6/scipy/sparse/sparsetools/csr_wrap.o" failed with exit status 1
```
### Trying to fix native Rascal build of Scipy ###

Still can't get sparse and special subpackages to build.

Attempting to run garbage collection more often, as suggested here: http://hostingfu.com/article/compiling-with-gcc-on-low-memory-vps
```sh
CFLAGS="$CFLAGS --param ggc-min-expand=0 --param ggc-min-heapsize=8192" python setup.py build
```

```sh
building 'scipy.sparse.sparsetools._csr' extension
compiling C++ sources
C compiler: arm-angstrom-linux-gnueabi-g++ -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -DNDEBUG -g -O3 -Wall --param ggc-min-expand=0 --param ggc-min-heapsize=8192 -fPIC

compile options: '-I/usr/lib/python2.6/site-packages/numpy/core/include -I/usr/include/python2.6 -c'
arm-angstrom-linux-gnueabi-g++: scipy/sparse/sparsetools/csr_wrap.cxx
arm-angstrom-linux-gnueabi-g++: Internal error: Killed (program cc1plus)
Please submit a full bug report.
See <http://gcc.gnu.org/bugs.html> for instructions.
arm-angstrom-linux-gnueabi-g++: Internal error: Killed (program cc1plus)
Please submit a full bug report.
See <http://gcc.gnu.org/bugs.html> for instructions.
error: Command "arm-angstrom-linux-gnueabi-g++ -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -DNDEBUG -g -O3 -Wall --param ggc-min-expand=0 --param ggc-min-heapsize=8192 -fPIC -I/usr/lib/python2.6/site-packages/numpy/core/include -I/usr/include/python2.6 -c scipy/sparse/sparsetools/csr_wrap.cxx -o build/temp.linux-armv5tejl-2.6/scipy/sparse/sparsetools/csr_wrap.o" failed with exit status 1
```
Also tried overriding -O3 with -O0, as http://stackoverflow.com/questions/6928110/how-may-i-override-the-compiler-gcc-flags-that-setup-py-uses-by-default

It does appear that the last setting on the command line is what takes effect, but the results are always the same, i.e. cc1plus still gets killed.

### Openembedded Scipy recipe ###

Need Numpy 1.4+, but Numpy 1.3.0 is installed on Ubuntu 10.04 LTS, and for some reason, the host library is used (via a symbolic link) by OE.

Not actually useful:
```sh
sudo apt-get install python-pip
```

Try installing 1.4.1 from source.
```sh
sudo apt-get remove python-numpy
sudo apt-get install python-dev
```

Strangely, building Numpy was easy.

```sh
python setup.py build
sudo python setup.py install
```

```sh
ubuntu@domU-12-31-39-0B-31-94:~$ python
Python 2.6.5 (r265:79063, Apr 16 2010, 13:09:56) 
[GCC 4.4.3] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import numpy as np
>>> np.__version__
'1.4.1'
```

Fix link
```sh
cd ~/openembedded-rascal/tmp/sysroots/i686-linux/usr/lib/python2.6
sudo ln -s /usr/local/lib/python2.6/dist-packages/numpy numpy
```

### Summary of Scipy progress, 2012-09-05 ###

* Got native compilation working on Rascal except for two subpackages (sparse and special), which take too much memory
* Scipy recipe starts to compile but then: scipy/special/c_misc/gammaincinv.c:1:20: error: Python.h: No such file or directory
* Also, BLAS and LAPACK are faked with empty files, but those libraries have been compiled on the Rascal.

### Building ATLAS ###

```sh
make[5]: Leaving directory `/home/root/ATLAS/rascal-atlas/tune/sysinfo'

      Read in L1 Cache size as = 32KB.
make[5]: Entering directory `/home/root/ATLAS/rascal-atlas/tune/sysinfo'
make RunMulAdd pre=s
make[6]: Entering directory `/home/root/ATLAS/rascal-atlas/tune/sysinfo'
/usr/bin/arm-angstrom-linux-gnueabi-gcc -DL2SIZE=4194304 -I/home/root/ATLAS/rascal-atlas/include -I/home/root/ATLAS/rascal-atlas/..//include -I/home/root/ATLAS/rascal-atlas/..//include/contrib -DAdd_ -DF77_INTEGER=int -DStringSunStyle -DATL_OS_Linux -DATL_GAS_ARM -DATL_DYLIBS -O -fomit-frame-pointer  -o xmasrch /home/root/ATLAS/rascal-atlas/..//tune/sysinfo/masrch.c
./xmasrch -p s -o res/sMULADD
Segmentation fault
make[7]: *** [xsma] Error 139
xmasrch: /home/root/ATLAS/rascal-atlas/..//tune/sysinfo/masrch.c:108: matime: Assertion `!system(ln)' failed.
Finding how many mflops required to get .025 second timings:
   1: 8.000000e-02
FORCE MFLOP=1, TIME=8.000000e-02

FINDING USABLE NREG:
     2: 11.88
     4: 12.00
     8: 11.76
    16: 11.88
    32: 11.65
    64: 11.88
make[6]: *** [RunMulAdd] Aborted
make[6]: Leaving directory `/home/root/ATLAS/rascal-atlas/tune/sysinfo'
make[5]: *** [res/sMULADD] Error 2
make[5]: Leaving directory `/home/root/ATLAS/rascal-atlas/tune/sysinfo'
xsyssum: /home/root/ATLAS/rascal-atlas/..//tune/sysinfo/GetSysSum.c:69: getfpinfo0: Assertion `system(fnam) == 0' failed.
make[4]: *** [/home/root/ATLAS/rascal-atlas/include/atlas_ssysinfo.h] Aborted
make[4]: Leaving directory `/home/root/ATLAS/rascal-atlas/tune/sysinfo'
make[3]: *** [/home/root/ATLAS/rascal-atlas/include/atlas_ssysinfo.h] Error 2
make[3]: Leaving directory `/home/root/ATLAS/rascal-atlas/src/auxil'
make[2]: *** [IStage1] Error 2
make[2]: Leaving directory `/home/root/ATLAS/rascal-atlas/bin'
ERROR 621 DURING CACHESIZE SEARCH!!.  CHECK INSTALL_LOG/Stage1.log FOR DETAILS.
make[2]: Entering directory `/home/root/ATLAS/rascal-atlas/bin'
cd /home/root/ATLAS/rascal-atlas ; make error_report
make[3]: Entering directory `/home/root/ATLAS/rascal-atlas'
make -f Make.top error_report
make[4]: Entering directory `/home/root/ATLAS/rascal-atlas'
uname -a 2>&1 >> bin/INSTALL_LOG/ERROR.LOG
/usr/bin/arm-angstrom-linux-gnueabi-gcc -v 2>&1  >> bin/INSTALL_LOG/ERROR.LOG
Using built-in specs.
Target: arm-angstrom-linux-gnueabi
Configured with: /home/ubuntu/openembedded-rascal/tmp/work/armv5te-angstrom-linux-gnueabi/gcc-4.3.3-r23.1/gcc-4.3.3/configure --build=i686-linux --host=arm-angstrom-linux-gnueabi --target=arm-angstrom-linux-gnueabi --prefix=/usr --exec_prefix=/usr --bindir=/usr/bin --sbindir=/usr/sbin --libexecdir=/usr/libexec --datadir=/usr/share --sysconfdir=/etc --sharedstatedir=/com --localstatedir=/var --libdir=/usr/lib --includedir=/usr/include --oldincludedir=/usr/include --infodir=/usr/share/info --mandir=/usr/share/man --enable-largefile --disable-nls --enable-ipv6 --with-gnu-ld --enable-shared --enable-languages=c,c++,objc,fortran --enable-threads=posix --disable-multilib --enable-c99 --enable-long-long --enable-symvers=gnu --enable-libstdcxx-pch --program-prefix=arm-angstrom-linux-gnueabi- --enable-target-optspace --enable-cheaders=c_std --enable-libssp --disable-bootstrap --disable-libgomp --disable-libmudflap --with-float=soft --with-local-prefix=/usr/local --with-gxx-include-dir=/usr/include/c++/4.3.3 --with-build-sysroot=/home/ubuntu/openembedded-rascal/tmp/sysroots/armv5te-angstrom-linux-gnueabi --enable-__cxa_atexit
Thread model: posix
gcc version 4.3.3 (GCC) 
/usr/bin/arm-angstrom-linux-gnueabi-gcc -V 2>&1  >> bin/INSTALL_LOG/ERROR.LOG
arm-angstrom-linux-gnueabi-gcc: '-V' option must have argument
make[4]: [error_report] Error 1 (ignored)
/usr/bin/arm-angstrom-linux-gnueabi-gcc --version 2>&1  >> bin/INSTALL_LOG/ERROR.LOG
tar cf error_UNKNOWN32.tar Make.inc bin/INSTALL_LOG/*
bzip2 error_UNKNOWN32.tar
make[4]: bzip2: Command not found
make[4]: *** [error_report] Error 127
make[4]: Leaving directory `/home/root/ATLAS/rascal-atlas'
make[3]: *** [error_report] Error 2
make[3]: Leaving directory `/home/root/ATLAS/rascal-atlas'
make[2]: *** [error_report] Error 2
make[2]: Leaving directory `/home/root/ATLAS/rascal-atlas/bin'
cat: can't open '../../CONFIG/error.txt': No such file or directory
cat: can't open '../../CONFIG/error.txt': No such file or directory
make[1]: *** [build] Error 255
make[1]: Leaving directory `/home/root/ATLAS/rascal-atlas'
make: *** [build] Error 2
```

```sh
opkg install bzip2
```

```sh
[root@rascal14:~/ATLAS]: mkdir rascal-atlas
[root@rascal14:~/ATLAS]: cd rascal-atlas/
[root@rascal14:~/ATLAS/rascal-atlas]: ../configure --shared
```

Building Scipy 0.8.0 crashes with error: Python.h: No such file or directory
```sh
ln -s armv5te-angstrom-linux-gnueabi arm-angstrom-linux-gnueabi
```
### Scipy build, overall strategy ###

Native build: won't build specfun.f (395 kB, largest .f file in Scipy), or csr_wrap.cxx. 
Cross-compile: uses host's cc and ld instead of cross-tools, which results in "Relocations in generic ELF" when trying to link fftpack.

### A/D converter shield ###

Candidate converters

AD1974: 4 ch, 24 bit, SPI, $9. Intended for audio. Max input range of 1.9 V. Maybe not a good choice.
AD7657: 6 ch, 14 bit, SPI, $19. Series: AD7656,7,8. Next gen product is AD7606,7,8. 20% more $, but full system.
AD7607: 8 ch, 14 bit, SPI, $15.29 in qty 100. (Appears to be pin-compatible with AD7606 (16 bit, $18.72) and AD7608 (18 bit, $26.43).)

AD760x uses 64-pin LQFP package, 0.50 mm pitch.