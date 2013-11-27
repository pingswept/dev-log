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
    cd meta-openembedded
    git branch -r
        origin/HEAD -> origin/master
        origin/danny
        origin/danny-next
        origin/denzil
        origin/denzil-next
        origin/dora
        origin/dylan
        origin/dylan-next
        origin/master
    git checkout -b dylan origin/dylan
        Branch dylan set up to track remote branch dylan from origin.
        Switched to a new branch 'dylan'

Add to bblayers.conf

    /home/ubuntu/poky/meta-openembedded/meta-oe \
    /home/ubuntu/poky/meta-openembedded/meta-systemd \

Switch Angstrom to Yocto 1.4 "Dylan" release

    git branch -r
        origin/HEAD -> origin/master
        origin/angstrom-v2012.05-yocto1.2
        origin/angstrom-v2012.12-yocto1.3
        origin/angstrom-v2013.06-yocto1.4
        origin/angstrom-v2013.12-yocto1.5
        origin/master
    git checkout -b dylan origin/angstrom-v2013.06-yocto1.4

Still fails

    bitbake core-image-minimal
    Loading cache: 100% |#########################################################################################################| ETA:  00:00:00Loaded 1662 entries from dependency cache.
    NOTE: ['/home/ubuntu/poky/meta-angstrom/recipes-tweaks/gstreamer/gstreamer_0.10.36.bbappend'] to []
    Parsing recipes: 100% |#######################################################################################################| Time: 00:00:03Parsing of 1298 .bb files complete (1275 cached, 23 parsed). 1660 targets, 55 skipped, 0 masked, 0 errors.
    ERROR: No recipes available for:
      /home/ubuntu/poky/meta-angstrom/recipes-tweaks/libgnome/libgnome_2.32.1.bbappend
      /home/ubuntu/poky/meta-angstrom/recipes-core/systemd/systemd_206.bbappend
      /home/ubuntu/poky/meta-angstrom/recipes-tweaks/connman/connman_1.17.bbappend
    ERROR: Command execution failed: Exited with 1

Suspect the problem is that Angstrom and dependencies are expecting Dylan 9.0.2 release, but meta-atmel layer instructions led me to check out 9.0.1 release.

    git checkout dylan-9.0.2 -b dylan-9.0.2
    source oe-init-build-env build-atmel
    bitbake core-image-minimal

Hmm. Same problem persists.

    Pseudo may be out of date, rebuilding pseudo before the main build
    Parsing recipes: 100% |#################################################################################| Time: 00:01:52
    Parsing of 1299 .bb files complete (0 cached, 1299 parsed). 1661 targets, 55 skipped, 0 masked, 0 errors.
    ERROR: No recipes available for:
      /home/ubuntu/poky/meta-angstrom/recipes-tweaks/libgnome/libgnome_2.32.1.bbappend
      /home/ubuntu/poky/meta-angstrom/recipes-core/systemd/systemd_206.bbappend
      /home/ubuntu/poky/meta-angstrom/recipes-tweaks/connman/connman_1.17.bbappend
    ERROR: Command execution failed: Exited with 1

Asked on Angstrom list: https://www.mail-archive.com/angstrom-distro-devel@linuxtogo.org/msg06767.html

Pull fixes connman problem

Then:

    mv systemd_206.bbappend systemd_199.bbappend
    rm -rf meta-angstrom/recipes-tweaks/libgnome
