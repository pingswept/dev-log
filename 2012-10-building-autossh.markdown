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
