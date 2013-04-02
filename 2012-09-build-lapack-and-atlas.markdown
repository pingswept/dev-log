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

### A/D converter shield ###

Candidate converters

AD1974: 4 ch, 24 bit, SPI, $9. Intended for audio. Max input range of 1.9 V. Maybe not a good choice.
AD7657: 6 ch, 14 bit, SPI, $19. Series: AD7656,7,8. Next gen product is AD7606,7,8. 20% more $, but full system.
AD7607: 8 ch, 14 bit, SPI, $15.29 in qty 100. (Appears to be pin-compatible with AD7606 (16 bit, $18.72) and AD7608 (18 bit, $26.43).)

AD760x uses 64-pin LQFP package, 0.50 mm pitch.

*Screw terminals*

0.100" pitch
* Phoenix Contact 1725711, 8 pos, Combicon MPT, $4.11 in qty 100, 277-1279-ND, green
* TE Connectivity 282834-8, 8 pos, Buchanan, $3.65 in qty. 100, A98338-ND, green
* Onshore Technology OSTVN08A150, 8 pos, $1.27 in qty. 100, ED10566-ND, green

3.5 mm pitch
* Phoenix Contact 1985250, 8 pos, Combicon PTSA, $0.73 in qty. 100, 277-1628-ND, green (funny footprint)
* Onshore Technology OSTTE080104, 8 pos, $0.86 in qty. 100, ED2746-ND, black

### Adding swap file ###

```sh
[root@rascal14:~]: dd if=/dev/zero of=/home/root/swap bs=1M count=100        
100+0 records in
100+0 records out
[root@rascal14:~]: chmod 0600 swap
[root@rascal14:~]: mkswap swap
Setting up swapspace version 1, size = 102396 KiB
no label, UUID=4b17557d-afce-4e73-aea7-5a124e92355b
[root@rascal14:~]: swapon swap
```

Useful refs:

* http://www.linuxquestions.org/questions/linux-general-1/2-6-3-swapon-function-not-implemented-155442/
* http://askubuntu.com/questions/126018/adding-a-new-swap-file-how-to-edit-fstab-to-enable-swap-after-reboot/126049#126049

### More Scipy cross-compilation ###

What I think the command line should look like, based on Numpy:

```sh
arm-angstrom-linux-gnueabi-gcc -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -DNDEBUG -g -O3 -Wall -Wstrict-prototypes -fexpensive-optimizations -frename-registers -fomit-frame-pointer -O2 -ggdb2 -fPIC
```

When building Scipy natively on the Rascal with 500 MB of swap space, this is what happens:

```sh
[root@rascal14:~/scipy-0.10.1]: python setup.py build
Running from scipy source directory.
blas_opt_info:
blas_mkl_info:
  libraries mkl,vml,guide not found in /usr/lib
  NOT AVAILABLE

atlas_blas_threads_info:
Setting PTATLAS=ATLAS
  libraries ptf77blas,ptcblas,atlas not found in /usr/lib
  NOT AVAILABLE

atlas_blas_info:
  libraries f77blas,cblas,atlas not found in /usr/lib
  NOT AVAILABLE

/usr/lib/python2.6/site-packages/numpy/distutils/system_info.py:1425: UserWarning: 
    Atlas (http://math-atlas.sourceforge.net/) libraries not found.
    Directories to search for the libraries can be specified in the
    numpy/distutils/site.cfg file (section [atlas]) or by setting
    the ATLAS environment variable.
  warnings.warn(AtlasNotFoundError.__doc__)
blas_info:
Replacing _lib_names[0]=='blas' with 'blas'
Replacing _lib_names[0]=='blas' with 'blas'
  FOUND:
    libraries = ['blas']
    library_dirs = ['/home/root']
    language = f77

  FOUND:
    libraries = ['blas']
    library_dirs = ['/home/root']
    define_macros = [('NO_ATLAS_INFO', 1)]
    language = f77

non-existing path in 'scipy/io': 'docs'
lapack_opt_info:
lapack_mkl_info:
mkl_info:
  libraries mkl,vml,guide not found in /usr/lib
  NOT AVAILABLE

  NOT AVAILABLE

atlas_threads_info:
Setting PTATLAS=ATLAS
  libraries ptf77blas,ptcblas,atlas not found in /usr/lib
  libraries lapack_atlas not found in /usr/lib
numpy.distutils.system_info.atlas_threads_info
  NOT AVAILABLE

atlas_info:
  libraries f77blas,cblas,atlas not found in /usr/lib
  libraries lapack_atlas not found in /usr/lib
numpy.distutils.system_info.atlas_info
  NOT AVAILABLE

/usr/lib/python2.6/site-packages/numpy/distutils/system_info.py:1340: UserWarning: 
    Atlas (http://math-atlas.sourceforge.net/) libraries not found.
    Directories to search for the libraries can be specified in the
    numpy/distutils/site.cfg file (section [atlas]) or by setting
    the ATLAS environment variable.
  warnings.warn(AtlasNotFoundError.__doc__)
lapack_info:
Replacing _lib_names[0]=='lapack' with 'lapack'
Replacing _lib_names[0]=='lapack' with 'lapack'
  FOUND:
    libraries = ['lapack']
    library_dirs = ['/home/root']
    language = f77

  FOUND:
    libraries = ['lapack', 'blas']
    library_dirs = ['/home/root']
    define_macros = [('NO_ATLAS_INFO', 1)]
    language = f77

umfpack_info:
  libraries umfpack not found in /usr/lib
/usr/lib/python2.6/site-packages/numpy/distutils/system_info.py:470: UserWarning: 
    UMFPACK sparse solver (http://www.cise.ufl.edu/research/sparse/umfpack/)
    not found. Directories to search for the libraries can be specified in the
    numpy/distutils/site.cfg file (section [umfpack]) or by setting
    the UMFPACK environment variable.
  warnings.warn(self.notfounderror.__doc__)
  NOT AVAILABLE

running build
running config_cc
unifing config_cc, config, build_clib, build_ext, build commands --compiler options
running config_fc
unifing config_fc, config, build_clib, build_ext, build commands --fcompiler options
running build_src
build_src
building py_modules sources
building library "dfftpack" sources
building library "fftpack" sources
building library "linpack_lite" sources
building library "mach" sources
building library "quadpack" sources
building library "odepack" sources
building library "dop" sources
building library "fitpack" sources
building library "odrpack" sources
building library "minpack" sources
building library "rootfind" sources
building library "superlu_src" sources
building library "arpack_scipy" sources
building library "qhull" sources
building library "sc_c_misc" sources
building library "sc_cephes" sources
building library "sc_mach" sources
building library "sc_toms" sources
building library "sc_amos" sources
building library "sc_cdf" sources
building library "sc_specfun" sources
building library "statlib" sources
building extension "scipy.cluster._vq" sources
building extension "scipy.cluster._hierarchy_wrap" sources
building extension "scipy.fftpack._fftpack" sources
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.fftpack.convolve" sources
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.integrate._quadpack" sources
building extension "scipy.integrate._odepack" sources
building extension "scipy.integrate.vode" sources
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.integrate._dop" sources
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.interpolate.interpnd" sources
building extension "scipy.interpolate._fitpack" sources
building extension "scipy.interpolate.dfitpack" sources
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
  adding 'build/src.linux-armv5tejl-2.6/scipy/interpolate/src/dfitpack-f2pywrappers.f' to sources.
building extension "scipy.interpolate._interpolate" sources
building extension "scipy.io.matlab.streams" sources
building extension "scipy.io.matlab.mio_utils" sources
building extension "scipy.io.matlab.mio5_utils" sources
building extension "scipy.lib.blas.fblas" sources
f2py options: ['skip:', ':']
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
  adding 'build/src.linux-armv5tejl-2.6/build/src.linux-armv5tejl-2.6/scipy/lib/blas/fblas-f2pywrappers.f' to sources.
building extension "scipy.lib.blas.cblas" sources
  adding 'build/src.linux-armv5tejl-2.6/scipy/lib/blas/cblas.pyf' to sources.
f2py options: ['skip:', ':']
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.lib.lapack.flapack" sources
f2py options: ['skip:', ':']
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.lib.lapack.clapack" sources
  adding 'build/src.linux-armv5tejl-2.6/scipy/lib/lapack/clapack.pyf' to sources.
f2py options: ['skip:', ':']
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.lib.lapack.calc_lwork" sources
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.lib.lapack.atlas_version" sources
building extension "scipy.linalg.fblas" sources
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
  adding 'build/src.linux-armv5tejl-2.6/build/src.linux-armv5tejl-2.6/scipy/linalg/fblas-f2pywrappers.f' to sources.
building extension "scipy.linalg.cblas" sources
  adding 'build/src.linux-armv5tejl-2.6/scipy/linalg/cblas.pyf' to sources.
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.linalg.flapack" sources
  adding 'build/src.linux-armv5tejl-2.6/scipy/linalg/flapack.pyf' to sources.
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
  adding 'build/src.linux-armv5tejl-2.6/build/src.linux-armv5tejl-2.6/scipy/linalg/flapack-f2pywrappers.f' to sources.
building extension "scipy.linalg.clapack" sources
  adding 'build/src.linux-armv5tejl-2.6/scipy/linalg/clapack.pyf' to sources.
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.linalg._flinalg" sources
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.linalg.calc_lwork" sources
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.linalg.atlas_version" sources
building extension "scipy.odr.__odrpack" sources
building extension "scipy.optimize._minpack" sources
building extension "scipy.optimize._zeros" sources
building extension "scipy.optimize._lbfgsb" sources
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.optimize.moduleTNC" sources
building extension "scipy.optimize._cobyla" sources
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.optimize.minpack2" sources
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.optimize._slsqp" sources
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.optimize._nnls" sources
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.signal.sigtools" sources
building extension "scipy.signal.spectral" sources
building extension "scipy.signal.spline" sources
building extension "scipy.sparse.linalg.isolve._iterative" sources
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.sparse.linalg.dsolve._superlu" sources
building extension "scipy.sparse.linalg.dsolve.umfpack.__umfpack" sources
building extension "scipy.sparse.linalg.eigen.arpack._arpack" sources
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
  adding 'build/src.linux-armv5tejl-2.6/build/src.linux-armv5tejl-2.6/scipy/sparse/linalg/eigen/arpack/_arpack-f2pywrappers.f' to sources.
building extension "scipy.sparse.sparsetools._csr" sources
building extension "scipy.sparse.sparsetools._csc" sources
building extension "scipy.sparse.sparsetools._coo" sources
building extension "scipy.sparse.sparsetools._bsr" sources
building extension "scipy.sparse.sparsetools._dia" sources
building extension "scipy.sparse.sparsetools._csgraph" sources
building extension "scipy.spatial.qhull" sources
building extension "scipy.spatial.ckdtree" sources
building extension "scipy.spatial._distance_wrap" sources
building extension "scipy.special._cephes" sources
building extension "scipy.special.specfun" sources
f2py options: ['--no-wrap-functions']
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.special.orthogonal_eval" sources
building extension "scipy.special.lambertw" sources
building extension "scipy.special._logit" sources
building extension "scipy.stats.statlib" sources
f2py options: ['--no-wrap-functions']
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.stats.vonmises_cython" sources
building extension "scipy.stats.futil" sources
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
building extension "scipy.stats.mvn" sources
f2py options: []
  adding 'build/src.linux-armv5tejl-2.6/fortranobject.c' to sources.
  adding 'build/src.linux-armv5tejl-2.6' to include_dirs.
  adding 'build/src.linux-armv5tejl-2.6/scipy/stats/mvn-f2pywrappers.f' to sources.
building extension "scipy.ndimage._nd_image" sources
building data_files sources
build_src: building npy-pkg config files
running build_py
copying scipy/setup.py -> build/lib.linux-armv5tejl-2.6/scipy
copying scipy/version.py -> build/lib.linux-armv5tejl-2.6/scipy
copying build/src.linux-armv5tejl-2.6/scipy/__config__.py -> build/lib.linux-armv5tejl-2.6/scipy
running build_clib
customize UnixCCompiler
customize UnixCCompiler using build_clib
customize GnuFCompiler
Could not locate executable g77
Could not locate executable f77
customize IntelFCompiler
Could not locate executable ifort
Could not locate executable ifc
customize LaheyFCompiler
Could not locate executable lf95
customize PGroupFCompiler
Could not locate executable pgfortran
customize AbsoftFCompiler
Could not locate executable f90
customize NAGFCompiler
Found executable /usr/bin/f95
customize VastFCompiler
customize CompaqFCompiler
Could not locate executable fort
customize IntelItaniumFCompiler
Could not locate executable efort
Could not locate executable efc
customize IntelEM64TFCompiler
customize Gnu95FCompiler
Found executable /usr/bin/gfortran
customize Gnu95FCompiler
customize Gnu95FCompiler using build_clib
running build_ext
customize UnixCCompiler
customize UnixCCompiler using build_ext
extending extension 'scipy.sparse.linalg.dsolve._superlu' defined_macros with [('USE_VENDOR_BLAS', 1)]
customize UnixCCompiler
customize UnixCCompiler using build_ext
customize GnuFCompiler
customize IntelFCompiler
customize LaheyFCompiler
customize PGroupFCompiler
customize AbsoftFCompiler
customize NAGFCompiler
customize VastFCompiler
customize CompaqFCompiler
customize IntelItaniumFCompiler
customize IntelEM64TFCompiler
customize Gnu95FCompiler
customize Gnu95FCompiler
customize Gnu95FCompiler using build_ext
building 'scipy.sparse.sparsetools._csr' extension
compiling C++ sources
C compiler: arm-angstrom-linux-gnueabi-g++ -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -fexpensive-optimizations -frename-registers -fomit-frame-pointer -O2 -ggdb2 -DNDEBUG -g -O3 -Wall -fPIC

compile options: '-D__STDC_FORMAT_MACROS=1 -I/usr/lib/python2.6/site-packages/numpy/core/include -I/usr/include/python2.6 -c'
arm-angstrom-linux-gnueabi-g++: scipy/sparse/sparsetools/csr_wrap.cxx
In file included from scipy/sparse/sparsetools/py3k.h:23,
                 from scipy/sparse/sparsetools/csr_wrap.cxx:2835:
/usr/lib/python2.6/site-packages/numpy/core/include/numpy/npy_3kcompat.h: In function 'PyObject* npy_PyFile_OpenFile(PyObject*, char*)':
/usr/lib/python2.6/site-packages/numpy/core/include/numpy/npy_3kcompat.h:258: warning: deprecated conversion from string constant to 'char*'
/usr/lib/python2.6/site-packages/numpy/core/include/numpy/npy_3kcompat.h: At global scope:
/usr/lib/python2.6/site-packages/numpy/core/include/numpy/npy_3kcompat.h:391: warning: 'void simple_capsule_dtor(void*)' defined but not used
arm-angstrom-linux-gnueabi-g++ -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -shared build/temp.linux-armv5tejl-2.6/scipy/sparse/sparsetools/csr_wrap.o -L/usr/lib -Lbuild/temp.linux-armv5tejl-2.6 -lpython2.6 -o build/lib.linux-armv5tejl-2.6/scipy/sparse/sparsetools/_csr.so
building 'scipy.sparse.sparsetools._csc' extension
compiling C++ sources
C compiler: arm-angstrom-linux-gnueabi-g++ -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -fexpensive-optimizations -frename-registers -fomit-frame-pointer -O2 -ggdb2 -DNDEBUG -g -O3 -Wall -fPIC

compile options: '-D__STDC_FORMAT_MACROS=1 -I/usr/lib/python2.6/site-packages/numpy/core/include -I/usr/include/python2.6 -c'
arm-angstrom-linux-gnueabi-g++: scipy/sparse/sparsetools/csc_wrap.cxx
In file included from scipy/sparse/sparsetools/py3k.h:23,
                 from scipy/sparse/sparsetools/csc_wrap.cxx:2821:
/usr/lib/python2.6/site-packages/numpy/core/include/numpy/npy_3kcompat.h: In function 'PyObject* npy_PyFile_OpenFile(PyObject*, char*)':
/usr/lib/python2.6/site-packages/numpy/core/include/numpy/npy_3kcompat.h:258: warning: deprecated conversion from string constant to 'char*'
/usr/lib/python2.6/site-packages/numpy/core/include/numpy/npy_3kcompat.h: At global scope:
/usr/lib/python2.6/site-packages/numpy/core/include/numpy/npy_3kcompat.h:391: warning: 'void simple_capsule_dtor(void*)' defined but not used
arm-angstrom-linux-gnueabi-g++: Internal error: Segmentation fault (program as)
Please submit a full bug report.
See <http://gcc.gnu.org/bugs.html> for instructions.
In file included from scipy/sparse/sparsetools/py3k.h:23,
                 from scipy/sparse/sparsetools/csc_wrap.cxx:2821:
/usr/lib/python2.6/site-packages/numpy/core/include/numpy/npy_3kcompat.h: In function 'PyObject* npy_PyFile_OpenFile(PyObject*, char*)':
/usr/lib/python2.6/site-packages/numpy/core/include/numpy/npy_3kcompat.h:258: warning: deprecated conversion from string constant to 'char*'
/usr/lib/python2.6/site-packages/numpy/core/include/numpy/npy_3kcompat.h: At global scope:
/usr/lib/python2.6/site-packages/numpy/core/include/numpy/npy_3kcompat.h:391: warning: 'void simple_capsule_dtor(void*)' defined but not used
arm-angstrom-linux-gnueabi-g++: Internal error: Segmentation fault (program as)
Please submit a full bug report.
See <http://gcc.gnu.org/bugs.html> for instructions.
error: Command "arm-angstrom-linux-gnueabi-g++ -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -fexpensive-optimizations -frename-registers -fomit-frame-pointer -O2 -ggdb2 -DNDEBUG -g -O3 -Wall -fPIC -D__STDC_FORMAT_MACROS=1 -I/usr/lib/python2.6/site-packages/numpy/core/include -I/usr/include/python2.6 -c scipy/sparse/sparsetools/csc_wrap.cxx -o build/temp.linux-armv5tejl-2.6/scipy/sparse/sparsetools/csc_wrap.o" failed with exit status 1
```

# Testing Edimax driver #

  [root@rascal14:/var/www]: lsusb   
  Bus 001 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
  Bus 001 Device 002: ID 7392:7811  
  [root@rascal14:/var/www]: insmod 8192cu.ko
  insmod: cannot insert '8192cu.ko': unknown symbol in module, or unknown parameter
  [root@rascal14:/var/www]: insmod 8192cu   
  insmod: cannot insert '8192cu': unknown symbol in module, or unknown parameter
  [root@rascal14:/var/www]: modprobe 8192cu
  [root@rascal14:/var/www]: lsmod
  Module                  Size  Used by    Not tainted
  zd1211rw               41875  0 
  v4l2_int_device         1974  0 
  v4l2_common             6297  0 
  uvcvideo               55337  0 
  videodev               60777  2 v4l2_common,uvcvideo
  v4l1_compat            13060  2 uvcvideo,videodev
  8192cu                523932  0

# Building Autossh with OpenEmbedded #

For native build, these are the commands that are executed. This creates a working executable.

    gcc -g -O2 -Wall -DVER=\"1.4c\" -DSSH_PATH=\"/usr/bin/ssh\"   -c -o autossh.o autossh.c
    gcc  -o autossh autossh.o -lnsl

Here's what the start of Makefile.in looks like after transformation via configure into Makefile:

    # $Id: Makefile.in,v 1.5 2011/10/12 20:29:22 harding Exp $
    #
    # Makefile.  Generated from Makefile.in by configure.
    
    VER=        1.4c
    
    SSH=        /usr/bin/ssh
    
    prefix=     /usr/local
    exec_prefix=    ${prefix}
    bindir=     ${exec_prefix}/bin
    datadir=    ${prefix}/share
    mandir=     ${prefix}/man
    
    SRCDIR=     .
    
    
    CC=     gcc
    CFLAGS=     -g -O2 -Wall -DVER=\"$(VER)\" -DSSH_PATH=\"$(SSH)\"
    CPPFLAGS=
    
    OFILES=     autossh.o
    
    LD=     @LD@
    LDFLAGS=
    LIBS=       -lnsl
    <snip>

### Files that need to be reuploaded to rm.com/files ###

deleting files/beriberi/nginx
deleting files/beriberi/modules
deleting files/beriberi/beriberi-binary-bundle-rc2-tar.gz
deleting files/beriberi/autossh_1.4c-r0.6_armv5te.ipk
deleting files/beriberi/

    usb 1-1: new full speed USB device using at91_ohci and address 2
    usb 1-1: USB disconnect, address 2
    usb 1-1: new full speed USB device using at91_ohci and address 3
    [root@rascal14:~]: lsusb -v
    
    Bus 001 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Device Descriptor:
      bLength                18
      bDescriptorType         1
      bcdUSB               1.10
      bDeviceClass            9 Hub
      bDeviceSubClass         0 Unused
      bDeviceProtocol         0 Full speed (or root) hub
      bMaxPacketSize0        64
      idVendor           0x1d6b Linux Foundation
      idProduct          0x0001 1.1 root hub
      bcdDevice            2.06
      iManufacturer           3 Linux 2.6.36+ ohci_hcd
      iProduct                2 AT91 OHCI
      iSerial                 1 at91
      bNumConfigurations      1
      Configuration Descriptor:
        bLength                 9
        bDescriptorType         2
        wTotalLength           25
        bNumInterfaces          1
        bConfigurationValue     1
        iConfiguration          0 
        bmAttributes         0xe0
          Self Powered
          Remote Wakeup
        MaxPower                0mA
        Interface Descriptor:
          bLength                 9
          bDescriptorType         4
          bInterfaceNumber        0
          bAlternateSetting       0
          bNumEndpoints           1
          bInterfaceClass         9 Hub
          bInterfaceSubClass      0 Unused
          bInterfaceProtocol      0 Full speed (or root) hub
          iInterface              0 
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x81  EP 1 IN
            bmAttributes            3
              Transfer Type            Interrupt
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x0002  1x 2 bytes
            bInterval             255
    Hub Descriptor:
      bLength               9
      bDescriptorType      41
      nNbrPorts             2
      wHubCharacteristic 0x0012
        No power switching (usb 1.0)
        No overcurrent protection
      bPwrOn2PwrGood        2 * 2 milli seconds
      bHubContrCurrent      0 milli Ampere
      DeviceRemovable    0x00
      PortPwrCtrlMask    0xff
     Hub Port Status:
       Port 1: 0000.0103 power enable connect
       Port 2: 0000.0100 power
    Device Status:     0x0003
      Self Powered
      Remote Wakeup Enabled
    
    Bus 001 Device 003: ID 1ffb:0089  
    Device Descriptor:
      bLength                18
      bDescriptorType         1
      bcdUSB               2.00
      bDeviceClass          239 Miscellaneous Device
      bDeviceSubClass         2 ?
      bDeviceProtocol         1 Interface Association
      bMaxPacketSize0         8
      idVendor           0x1ffb 
      idProduct          0x0089 
      bcdDevice            1.01
      iManufacturer           1 Pololu Corporation
      iProduct                2 Pololu Micro Maestro 6-Servo Controller
      iSerial                 5 00039612
      bNumConfigurations      1
      Configuration Descriptor:
        bLength                 9
        bDescriptorType         2
        wTotalLength          150
        bNumInterfaces          5
        bConfigurationValue     1
        iConfiguration          0 
        bmAttributes         0xc0
          Self Powered
        MaxPower              100mA
        Interface Association:
          bLength                 8
          bDescriptorType        11
          bFirstInterface         0
          bInterfaceCount         2
          bFunctionClass          2 Communications
          bFunctionSubClass       2 Abstract (modem)
          bFunctionProtocol       1 AT-commands (v.25ter)
          iFunction               3 Pololu Micro Maestro 6-Servo Controller Command Port
        Interface Descriptor:
          bLength                 9
          bDescriptorType         4
          bInterfaceNumber        0
          bAlternateSetting       0
          bNumEndpoints           1
          bInterfaceClass         2 Communications
          bInterfaceSubClass      2 Abstract (modem)
          bInterfaceProtocol      1 AT-commands (v.25ter)
          iInterface              0 
          CDC Header:
            bcdCDC               1.20
          CDC ACM:
            bmCapabilities       0x02
              line coding and serial state
          CDC Union:
            bMasterInterface        0
            bSlaveInterface         1 
          CDC Call Management:
            bmCapabilities       0x00
            bDataInterface          1
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x81  EP 1 IN
            bmAttributes            3
              Transfer Type            Interrupt
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x000a  1x 10 bytes
            bInterval               1
        Interface Descriptor:
          bLength                 9
          bDescriptorType         4
          bInterfaceNumber        1
          bAlternateSetting       0
          bNumEndpoints           2
          bInterfaceClass        10 CDC Data
          bInterfaceSubClass      0 Unused
          bInterfaceProtocol      0 
          iInterface              0 
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x02  EP 2 OUT
            bmAttributes            2
              Transfer Type            Bulk
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x0008  1x 8 bytes
            bInterval               0
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x82  EP 2 IN
            bmAttributes            2
              Transfer Type            Bulk
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x0008  1x 8 bytes
            bInterval               0
        Interface Association:
          bLength                 8
          bDescriptorType        11
          bFirstInterface         2
          bInterfaceCount         2
          bFunctionClass          2 Communications
          bFunctionSubClass       2 Abstract (modem)
          bFunctionProtocol       1 AT-commands (v.25ter)
          iFunction               4 Pololu Micro Maestro 6-Servo Controller TTL Port
        Interface Descriptor:
          bLength                 9
          bDescriptorType         4
          bInterfaceNumber        2
          bAlternateSetting       0
          bNumEndpoints           1
          bInterfaceClass         2 Communications
          bInterfaceSubClass      2 Abstract (modem)
          bInterfaceProtocol      1 AT-commands (v.25ter)
          iInterface              0 
          CDC Header:
            bcdCDC               1.20
          CDC ACM:
            bmCapabilities       0x02
              line coding and serial state
          CDC Union:
            bMasterInterface        2
            bSlaveInterface         3 
          CDC Call Management:
            bmCapabilities       0x00
            bDataInterface          3
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x83  EP 3 IN
            bmAttributes            3
              Transfer Type            Interrupt
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x000a  1x 10 bytes
            bInterval               1
        Interface Descriptor:
          bLength                 9
          bDescriptorType         4
          bInterfaceNumber        3
          bAlternateSetting       0
          bNumEndpoints           2
          bInterfaceClass        10 CDC Data
          bInterfaceSubClass      0 Unused
          bInterfaceProtocol      0 
          iInterface              0 
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x04  EP 4 OUT
            bmAttributes            2
              Transfer Type            Bulk
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x0008  1x 8 bytes
            bInterval               0
          Endpoint Descriptor:
            bLength                 7
            bDescriptorType         5
            bEndpointAddress     0x84  EP 4 IN
            bmAttributes            2
              Transfer Type            Bulk
              Synch Type               None
              Usage Type               Data
            wMaxPacketSize     0x0008  1x 8 bytes
            bInterval               0
        Interface Descriptor:
          bLength                 9
          bDescriptorType         4
          bInterfaceNumber        4
          bAlternateSetting       0
          bNumEndpoints           0
          bInterfaceClass       255 Vendor Specific Class
          bInterfaceSubClass      4 
          bInterfaceProtocol      1 
          iInterface              2 Pololu Micro Maestro 6-Servo Controller
    Device Status:     0x0000
      (Bus Powered)
    [root@rascal14:~]: lsusb
    Bus 001 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 001 Device 003: ID 1ffb:0089

### uWSGI on Linode ###

uwsgi --master --http 0.0.0.0:8080 --plugin python --file /var/www/ip-app.py --callable app
