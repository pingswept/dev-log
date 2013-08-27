Install `autoconf`

  opkg install autoconf
  Installing autoconf (2.65-r9.0.6) to root...
  Downloading http://rascalmicro.com/packages/beta/armv5te/base/autoconf_2.65-r9.0.6_armv5te.ipk.
  Installing m4 (1.4.14-r0.1.6) to root...
  Downloading http://rascalmicro.com/packages/beta/armv5te/base/m4_1.4.14-r0.1.6_armv5te.ipk.
  Installing gnu-config (git-r0+gitre35217687ee5f39b428119fe31c7e954f6de64f0.6) to root...
  Downloading http://rascalmicro.com/packages/beta/armv5te/base/gnu-config_git-r0+gitre35217687ee5f39b428119fe31c7e954f6de64f0.6_armv5te.ipk.
  Configuring m4.
  Configuring gnu-config.
  Configuring autoconf.

Use `autoconf` to process `configure.ac`

  [root@rascal14$:~/alsaplayer-master]: autoconf
  configure.ac:12: error: possibly undefined macro: AM_INIT_AUTOMAKE
        If this token and others are legitimate, please use m4_pattern_allow.
        See the Autoconf documentation.
  configure.ac:13: error: possibly undefined macro: AM_SILENT_RULES
  configure.ac:25: error: possibly undefined macro: AM_GNU_GETTEXT_VERSION
  configure.ac:26: error: possibly undefined macro: AM_GNU_GETTEXT
  configure.ac:30: error: possibly undefined macro: AM_DISABLE_STATIC
  configure.ac:31: error: possibly undefined macro: AM_PROG_LIBTOOL
  configure.ac:32: error: possibly undefined macro: AM_PROG_CC_C_O
  configure.ac:119: error: possibly undefined macro: AM_PATH_LIBMIKMOD
  configure.ac:120: error: possibly undefined macro: AM_PATH_OGG
  configure.ac:121: error: possibly undefined macro: AM_PATH_VORBIS
  configure.ac:180: error: possibly undefined macro: AM_CONDITIONAL
  configure.ac:375: error: possibly undefined macro: AM_PATH_ESD
