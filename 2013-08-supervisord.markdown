Package install appears to fail, but it's actually just the post-install configure that fails.

    wget http://rascalmicro.com/files/supervisord_3.0-ml1.6_armv5te.ipk
    Connecting to rascalmicro.com (69.164.219.36:80)
    supervisord_3.0-ml1. 100% |*************************************************************|   233k --:--:-- ETA
    [root@rascal166$:~]: opkg install supervisord_3.0-ml1.6_armv5te.ipk 
    Installing supervisord (3.0-ml1.6) to root...
    Configuring supervisord.
    update-rc.d: /etc/init.d/supervisord: file does not exist
    Collected errors:
     * pkg_run_script: package "supervisord" postinst script returned status 1.
     * opkg_configure: supervisord.postinst returned 1.
    [root@rascal166$:~]: opkg install supervisord
    Package supervisord (3.0-ml1.6) installed in root is up to date.
    Configuring supervisord.
    update-rc.d: /etc/init.d/supervisord: file does not exist
    Collected errors:
     * pkg_run_script: package "supervisord" postinst script returned status 1.
     * opkg_configure: supervisord.postinst returned 1.

Depends on `meld3` (should be built by OE, but isn't)

    [root@rascal166$:~]: supervisord
    Traceback (most recent call last):
      File "/usr/bin/supervisord", line 5, in <module>
        from pkg_resources import load_entry_point
      File "build/bdist.linux-i686/egg/pkg_resources.py", line 2603, in <module>
      File "build/bdist.linux-i686/egg/pkg_resources.py", line 666, in require
      File "build/bdist.linux-i686/egg/pkg_resources.py", line 565, in resolve
    pkg_resources.DistributionNotFound: meld3>=0.6.5
    [root@rascal166$:~]: pip install meld3
    Downloading/unpacking meld3
      Downloading meld3-0.6.10.tar.gz (41Kb): 41Kb downloaded
      Running setup.py egg_info for package meld3
    Installing collected packages: meld3
      Running setup.py install for meld3
        /usr/bin/python /tmp/tmpgN4cXW.py
        removing /tmp/tmpgN4cXW.py
    Successfully installed meld3
    Cleaning up...

Successful invocation

    supervisord -c /etc/supervisord.conf
