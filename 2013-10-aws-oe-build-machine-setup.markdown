    sudo apt-get update
    sudo apt-get install build-essential chrpath cvs diffstat gawk git-core libglu1-mesa-dev libgl1-mesa-dev libsdl1.2-dev subversion texinfo texi2html

    git clone git://git.yoctoproject.org/poky
    cd poky
    git checkout dylan-9.0.1 -b rascal
    git clone http://github.com/linux4sam/meta-atmel
    source oe-init-build-env build-atmel

Add to BBLAYERS in conf/bblayers.conf:

    /home/ubuntu/poky/meta-atmel \

Edit conf/local.conf:

    MACHINE ??= "sama5d3xek" # was "qemux86"
    [...]
    PACKAGE_CLASSES ?= "package_ipk" # was "package_rpm"

Build

    bitbake core-image-minimal

Appears to have worked!

    Build Configuration:
    BB_VERSION        = "1.18.0"
    BUILD_SYS         = "x86_64-linux"
    NATIVELSBSTRING   = "Ubuntu-12.04"
    TARGET_SYS        = "arm-poky-linux-gnueabi"
    MACHINE           = "sama5d3xek"
    DISTRO            = "poky"
    DISTRO_VERSION    = "1.4.1"
    TUNE_FEATURES     = "armv7a vfp thumb callconvention-hard cortexa9"
    TARGET_FPU        = "vfp"
    meta-atmel        = "master:cf93c5213df976be11416a2857e62738175f8f04"
    meta
    meta-yocto
    meta-yocto-bsp    = "rascal:73f103bf9b2cdf985464dc53bf4f1cfd71d4531f"
    
    NOTE: Resolving any missing task queue dependencies
    NOTE: Preparing runqueue
    NOTE: Executing SetScene Tasks
    NOTE: Executing RunQueue Tasks
    WARNING: Failed to fetch URL http://www.zlib.net/zlib-1.2.7.tar.bz2, attempting MIRRORS if available
    WARNING: Failed to fetch URL ftp://ftp.ossp.org/pkg/lib/uuid/uuid-1.6.2.tar.gz, attempting MIRRORS if available
    WARNING: Failed to fetch URL http://zlib.net/pigz/pigz-2.3.tar.gz, attempting MIRRORS if available
    WARNING: Failed to fetch URL http://www.apache.org/dist/apr/apr-util-1.5.1.tar.gz, attempting MIRRORS if available
    WARNING: Failed to fetch URL http://www.apache.org/dist/subversion/subversion-1.7.8.tar.bz2, attempting MIRRORS if available
    NOTE: validating kernel config, see log.do_kernel_configcheck for details
    WARNING: Failed to fetch URL http://www.angstrom-distribution.org/unstable/sources/tinylogin-1.4.tar.bz2, attempting MIRRORS if available
    NOTE: Tasks Summary: Attempted 1543 tasks of which 306 didn't need to be rerun and all succeeded.

### Adding Angstrom ###

Clone repo into poky/meta-angstrom

    cd ~/poky
    git clone https://github.com/Angstrom-distribution/meta-angstrom.git

Add to BBLAYERS in conf/bblayers.conf:

    /home/ubuntu/poky/meta-angstrom \

Build fails.

    ERROR: No recipes available for:
      /home/ubuntu/poky/meta-angstrom/recipes-core/systemd/systemd_206.bbappend
      /home/ubuntu/poky/meta-angstrom/recipes-tweaks/connman/connman_1.19.bbappend
    ERROR: Command execution failed: Exited with 1

Seems we also need to manually add dependencies, meta-oe and meta-systemd, both of which are subfolders of the meta-openembedded repo.

Clone the repo

    git clone git://git.openembedded.org/meta-openembedded

Add to bblayers.conf

    /home/ubuntu/poky/meta-openembedded/meta-oe \
    /home/ubuntu/poky/meta-openembedded/meta-systemd \

Results in Kerberos 5 build error

    ERROR: Error executing a python function in <code>:                                                                         | ETA:  00:01:17
    ExpansionError: Failure expanding variable SHRT_VER, expression was ${@oe.utils.trim_version("1.11.3", 2)} which triggered exception AttributeError: 'module' object has no attribute 'trim_version'
    
    ERROR: The stack trace of python calls that resulted in this exception/failure was:
    ERROR:   File "<code>", line 1, in <module>
    ERROR: 
    ERROR:   File "__anon_635__home_ubuntu_poky_meta_classes_base_bbclass", line 169, in __anon_635__home_ubuntu_poky_meta_classes_base_bbclass
    ERROR: 
    ERROR:   File "/home/ubuntu/poky/bitbake/lib/bb/data_smart.py", line 503, in getVar
    ERROR:     return self.expand(value, var)
    ERROR: 
    ERROR:   File "/home/ubuntu/poky/bitbake/lib/bb/data_smart.py", line 336, in expand
    ERROR:     return self.expandWithRefs(s, varname).value
    ERROR: 
    ERROR:   File "/home/ubuntu/poky/bitbake/lib/bb/data_smart.py", line 317, in expandWithRefs
    ERROR:     s = __expand_var_regexp__.sub(varparse.var_sub, s)
    ERROR: 
    ERROR:   File "/home/ubuntu/poky/bitbake/lib/bb/data_smart.py", line 97, in var_sub
    ERROR:     var = self.d.getVar(key, True)
    ERROR: 
    ERROR:   File "/home/ubuntu/poky/bitbake/lib/bb/data_smart.py", line 503, in getVar
    ERROR:     return self.expand(value, var)
    ERROR: 
    ERROR:   File "/home/ubuntu/poky/bitbake/lib/bb/data_smart.py", line 336, in expand
    ERROR:     return self.expandWithRefs(s, varname).value
    ERROR: 
    ERROR:   File "/home/ubuntu/poky/bitbake/lib/bb/data_smart.py", line 326, in expandWithRefs
    ERROR:     raise ExpansionError(varname, s, exc)
    ERROR: 
    ERROR: The code that was being executed was:
    ERROR:  *** 0001:__anon_635__home_ubuntu_poky_meta_classes_base_bbclass(d)
    ERROR:      0002:__anon_227__home_ubuntu_poky_meta_classes_package_bbclass(d)
    ERROR:      0003:__anon_420__home_ubuntu_poky_meta_classes_package_ipk_bbclass(d)
    ERROR:      0004:__anon_914__home_ubuntu_poky_meta_classes_insane_bbclass(d)
    ERROR:      0005:__anon_20__home_ubuntu_poky_meta_classes_debian_bbclass(d)
    ERROR: [From file: '<code>', lineno: 1, function: <module>]
    ERROR:      0165:                elif all_skipped or incompatible_license(d, bad_licenses):
    ERROR:      0166:                    bb.debug(1, "SKIPPING recipe %s because it's %s" % (pn, recipe_license))
    ERROR:      0167:                    raise bb.parse.SkipPackage("incompatible with license %s" % recipe_license)
    ERROR:      0168:
    ERROR:  *** 0169:    srcuri = d.getVar('SRC_URI', True)
    ERROR:      0170:    # Svn packages should DEPEND on subversion-native
    ERROR:      0171:    if "svn://" in srcuri:
    ERROR:      0172:        d.appendVarFlag('do_fetch', 'depends', ' subversion-native:do_populate_sysroot')
    ERROR:      0173:
    ERROR: [From file: '__anon_635__home_ubuntu_poky_meta_classes_base_bbclass', lineno: 169, function: __anon_635__home_ubuntu_poky_meta_classes_base_bbclass]
    ERROR: Failed to parse recipe: /home/ubuntu/poky/meta-openembedded/meta-oe/recipes-connectivity/krb5/krb5_1.11.3.bb
    ERROR: Command execution failed: Exited with 1
