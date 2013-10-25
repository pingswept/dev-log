
    sudo apt-get update
    sudo apt-get install build-essential chrpath cvs diffstat gawk git-core libglu1-mesa-dev libgl1-mesa-dev libsdl1.2-dev subversion texinfo texi2html

    git clone git://git.yoctoproject.org/poky
    cd poky
    git checkout dylan-9.0.1 -b rascal
    git clone http://github.com/linux4sam/meta-atmel
