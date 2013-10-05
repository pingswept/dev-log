uwsgi --http 0.0.0.0:5000 --ini public.ini

uwsgi --https :8443,foobar.crt,foobar.key --http-raw-body --gevent 100 --module public --callable public

uwsgi --http 0.0.0.0:5000 --static-map /favicon.ico=/var/www/public/static/images/led.gif --ini public.ini

### Summary of the situation ###

uWSGI runs the websockets demo, but the demo doesn't actually work without gevent 1.x or newer.

To build gevent 1.x, we need Cython.

I successfully backported a recipe for Cython 0.15.1 to the Rascal OE tree, built a Cython package, and tried to build gevent 1.0rc2.

Here is where my troubles began:

    [root@rascal14$:/var/www/gevent-1.0rc2]: python setup.py install
    running install
    running bdist_egg
    running egg_info
    writing requirements to gevent.egg-info/requires.txt
    writing gevent.egg-info/PKG-INFO
    writing top-level names to gevent.egg-info/top_level.txt
    writing dependency_links to gevent.egg-info/dependency_links.txt
    reading manifest file 'gevent.egg-info/SOURCES.txt'
    reading manifest template 'MANIFEST.in'
    warning: no files found matching 'Makefile.ext'
    writing manifest file 'gevent.egg-info/SOURCES.txt'
    installing library code to build/bdist.linux-armv5tejl/egg
    running install_lib
    running build_py
    running build_ext
    /usr/bin/python util/cythonpp.py -o gevent.core.c gevent/core.ppyx
    Running cython -o gevent.core.c gevent/core.pyx  # !EV_USE_SIGNALFD && !defined(LIBEV_EMBED) && !defined(_WIN32)
    Running cython -o gevent.core.c gevent/core.pyx  # !EV_USE_SIGNALFD && !defined(LIBEV_EMBED) && defined(_WIN32)
    Running cython -o gevent.core.c gevent/core.pyx  # !EV_USE_SIGNALFD && defined(LIBEV_EMBED) && !defined(_WIN32)
    Traceback (most recent call last):
        File "util/cythonpp.py", line 801, in <module>
            process_filename(filename, options.output_file)
        File "util/cythonpp.py", line 85, in process_filename
            output = run_cython(pyx_filename, sourcehash, output_filename, banner, comment)
        File "util/cythonpp.py", line 529, in run_cython
            system(command, comment)
        File "util/cythonpp.py", line 539, in system
            raise AssertionError('%r failed with code %s' % (command, result))
    AssertionError: 'cython -o gevent.core.c gevent/core.pyx' failed with code -1
    make: *** [gevent/gevent.core.c] Error 1

### Tried building on Ubuntu 12.04 desktop machine ###

sudo pip installed Cython-0.19.1.

Then successfully built gevent-1.0b4, as seen from the spew below.


    ➜  ~  cd gevent-1.0b4 
    ➜  gevent-1.0b4  sudo python setup.py install
    running install
    Checking .pth file support in /usr/local/lib/python2.7/dist-packages/
    /usr/bin/python -E -c pass
    TEST PASSED: /usr/local/lib/python2.7/dist-packages/ appears to support .pth files
    running bdist_egg
    running egg_info
    creating gevent.egg-info
    writing requirements to gevent.egg-info/requires.txt
    writing gevent.egg-info/PKG-INFO
    writing top-level names to gevent.egg-info/top_level.txt
    writing dependency_links to gevent.egg-info/dependency_links.txt
    writing manifest file 'gevent.egg-info/SOURCES.txt'
    reading manifest file 'gevent.egg-info/SOURCES.txt'
    reading manifest template 'MANIFEST.in'
    warning: no files found matching 'Makefile.ext'
    writing manifest file 'gevent.egg-info/SOURCES.txt'
    installing library code to build/bdist.linux-i686/egg
    running install_lib
    running build_py
    creating build
    creating build/lib.linux-i686-2.7
    creating build/lib.linux-i686-2.7/gevent
    copying gevent/threadpool.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/subprocess.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/ssl.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/thread.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/local.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/hub.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/greenlet.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/server.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/resolver_ares.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/timeout.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/queue.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/lock.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/fileobject.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/_threading.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/wsgi.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/event.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/pywsgi.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/os.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/win32util.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/resolver_thread.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/select.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/backdoor.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/monkey.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/baseserver.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/util.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/coros.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/pool.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/socket.py -> build/lib.linux-i686-2.7/gevent
    copying gevent/__init__.py -> build/lib.linux-i686-2.7/gevent
    running build_ext
    /usr/bin/python util/cythonpp.py -o gevent.core.c gevent/core.ppyx
    Running cython -o gevent.core.c gevent/core.pyx  # !EV_CHILD_ENABLE && !EV_USE_SIGNALFD && !defined(LIBEV_EMBED) && !defined(_WIN32)
    Running cython -o gevent.core.c gevent/core.pyx  # !EV_CHILD_ENABLE && !EV_USE_SIGNALFD && !defined(LIBEV_EMBED) && defined(_WIN32)
    Running cython -o gevent.core.c gevent/core.pyx  # !EV_CHILD_ENABLE && !EV_USE_SIGNALFD && defined(LIBEV_EMBED) && !defined(_WIN32)
    Running cython -o gevent.core.c gevent/core.pyx  # !EV_CHILD_ENABLE && !EV_USE_SIGNALFD && defined(LIBEV_EMBED) && defined(_WIN32)
    Running cython -o gevent.core.c gevent/core.pyx  # !EV_CHILD_ENABLE && EV_USE_SIGNALFD && defined(LIBEV_EMBED) && !defined(_WIN32)
    Running cython -o gevent.core.c gevent/core.pyx  # !EV_CHILD_ENABLE && EV_USE_SIGNALFD && defined(LIBEV_EMBED) && defined(_WIN32)
    Reusing cython -o gevent.core.c gevent/core.pyx  # !EV_CHILD_ENABLE && EV_USE_SIGNALFD && defined(LIBEV_EMBED) && !defined(_WIN32)
    Reusing cython -o gevent.core.c gevent/core.pyx  # !EV_CHILD_ENABLE && EV_USE_SIGNALFD && defined(LIBEV_EMBED) && defined(_WIN32)
    Running cython -o gevent.core.c gevent/core.pyx  # EV_CHILD_ENABLE && !EV_USE_SIGNALFD && !defined(LIBEV_EMBED) && !defined(_WIN32)
    Running cython -o gevent.core.c gevent/core.pyx  # EV_CHILD_ENABLE && !EV_USE_SIGNALFD && !defined(LIBEV_EMBED) && defined(_WIN32)
    Running cython -o gevent.core.c gevent/core.pyx  # EV_CHILD_ENABLE && !EV_USE_SIGNALFD && defined(LIBEV_EMBED) && !defined(_WIN32)
    Running cython -o gevent.core.c gevent/core.pyx  # EV_CHILD_ENABLE && !EV_USE_SIGNALFD && defined(LIBEV_EMBED) && defined(_WIN32)
    Running cython -o gevent.core.c gevent/core.pyx  # EV_CHILD_ENABLE && EV_USE_SIGNALFD && defined(LIBEV_EMBED) && !defined(_WIN32)
    Running cython -o gevent.core.c gevent/core.pyx  # EV_CHILD_ENABLE && EV_USE_SIGNALFD && defined(LIBEV_EMBED) && defined(_WIN32)
    Reusing cython -o gevent.core.c gevent/core.pyx  # EV_CHILD_ENABLE && EV_USE_SIGNALFD && defined(LIBEV_EMBED) && !defined(_WIN32)
    Reusing cython -o gevent.core.c gevent/core.pyx  # EV_CHILD_ENABLE && EV_USE_SIGNALFD && defined(LIBEV_EMBED) && defined(_WIN32)
    Generating gevent.core.c 1858374 bytes
    Saving gevent/core.pyx
    echo                          >> gevent.core.c
    echo '#include "callbacks.c"' >> gevent.core.c
    mv gevent.core.* gevent/
    cython -o gevent.ares.c gevent/ares.pyx
    mv gevent.ares.* gevent/
    cython -o gevent._semaphore.c gevent/_semaphore.pyx
    mv gevent._semaphore.* gevent/
    cython -o gevent._util.c gevent/_util.pyx
    mv gevent._util.* gevent/
    Running '/bin/sh /home/brandon/gevent-1.0b4/libev/configure > configure-output.txt' in /home/brandon/gevent-1.0b4/build/temp.linux-i686-2.7/libev
    building 'gevent.core' extension
    creating build/temp.linux-i686-2.7/gevent
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DLIBEV_EMBED=1 -DEV_COMMON= -DEV_CHECK_ENABLE=0 -DEV_CLEANUP_ENABLE=0 -DEV_EMBED_ENABLE=0 -DEV_PERIODIC_ENABLE=0 -Ibuild/temp.linux-i686-2.7/libev -Ilibev -I/usr/include/python2.7 -c gevent/gevent.core.c -o build/temp.linux-i686-2.7/gevent/gevent.core.o
    In file included from gevent/libev.h:2:0,
                     from gevent/gevent.core.c:313:
    libev/ev.c:467:48: warning: "/*" within comment [-Wcomment]
    In file included from gevent/libev.h:2:0,
                     from gevent/gevent.core.c:313:
    libev/ev.c:1311:31: warning: ‘ev_default_loop_ptr’ initialized and declared ‘extern’ [enabled by default]
    In file included from gevent/libev.h:2:0,
                     from gevent/gevent.core.c:313:
    libev/ev.c: In function ‘ev_io_start’:
    libev/ev.c:3332:3: warning: suggest parentheses around arithmetic in operand of ‘|’ [-Wparentheses]
    In file included from gevent/libev.h:2:0,
                     from gevent/gevent.core.c:313:
    libev/ev.c: At top level:
    libev/ev.c:4563:27: warning: "/*" within comment [-Wcomment]
    libev/ev.c:4564:27: warning: "/*" within comment [-Wcomment]
    gevent/gevent.core.c: In function ‘evpipe_write’:
    libev/ev.c:1940:17: warning: ignoring return value of ‘write’, declared with attribute warn_unused_result [-Wunused-result]
    libev/ev.c:1952:17: warning: ignoring return value of ‘write’, declared with attribute warn_unused_result [-Wunused-result]
    gevent/gevent.core.c: In function ‘pipecb’:
    libev/ev.c:1973:16: warning: ignoring return value of ‘read’, declared with attribute warn_unused_result [-Wunused-result]
    libev/ev.c:1987:16: warning: ignoring return value of ‘read’, declared with attribute warn_unused_result [-Wunused-result]
    gcc -pthread -shared -Wl,-O1 -Wl,-Bsymbolic-functions -Wl,-Bsymbolic-functions -Wl,-z,relro build/temp.linux-i686-2.7/gevent/gevent.core.o -o build/lib.linux-i686-2.7/gevent/core.so
    Linking /home/brandon/gevent-1.0b4/build/lib.linux-i686-2.7/gevent/core.so to /home/brandon/gevent-1.0b4/gevent/core.so
    Running '/bin/sh /home/brandon/gevent-1.0b4/c-ares/configure CONFIG_COMMANDS= CONFIG_FILES= > configure-output.txt' in /home/brandon/gevent-1.0b4/build/temp.linux-i686-2.7/c-ares
    building 'gevent.ares' extension
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c gevent/gevent.ares.c -o build/temp.linux-i686-2.7/gevent/gevent.ares.o
    In file included from gevent/gevent.ares.c:314:0:
    gevent/dnshelper.c: In function ‘gevent_append_addr’:
    gevent/dnshelper.c:51:5: warning: implicit declaration of function ‘inet_ntop’ [-Wimplicit-function-declaration]
    gevent/dnshelper.c: In function ‘gevent_make_sockaddr’:
    gevent/dnshelper.c:137:5: warning: implicit declaration of function ‘inet_pton’ [-Wimplicit-function-declaration]
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares__close_sockets.c -o build/temp.linux-i686-2.7/c-ares/ares__close_sockets.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares__get_hostent.c -o build/temp.linux-i686-2.7/c-ares/ares__get_hostent.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares__read_line.c -o build/temp.linux-i686-2.7/c-ares/ares__read_line.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares__timeval.c -o build/temp.linux-i686-2.7/c-ares/ares__timeval.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_data.c -o build/temp.linux-i686-2.7/c-ares/ares_data.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_destroy.c -o build/temp.linux-i686-2.7/c-ares/ares_destroy.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_expand_name.c -o build/temp.linux-i686-2.7/c-ares/ares_expand_name.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_free_hostent.c -o build/temp.linux-i686-2.7/c-ares/ares_free_hostent.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_free_string.c -o build/temp.linux-i686-2.7/c-ares/ares_free_string.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_gethostbyaddr.c -o build/temp.linux-i686-2.7/c-ares/ares_gethostbyaddr.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_gethostbyname.c -o build/temp.linux-i686-2.7/c-ares/ares_gethostbyname.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_getnameinfo.c -o build/temp.linux-i686-2.7/c-ares/ares_getnameinfo.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_init.c -o build/temp.linux-i686-2.7/c-ares/ares_init.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_library_init.c -o build/temp.linux-i686-2.7/c-ares/ares_library_init.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_llist.c -o build/temp.linux-i686-2.7/c-ares/ares_llist.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_mkquery.c -o build/temp.linux-i686-2.7/c-ares/ares_mkquery.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_nowarn.c -o build/temp.linux-i686-2.7/c-ares/ares_nowarn.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_options.c -o build/temp.linux-i686-2.7/c-ares/ares_options.o
    c-ares/ares_options.c: In function ‘ares_set_servers_csv’:
    c-ares/ares_options.c:187:9: warning: ignoring return value of ‘strtol’, declared with attribute warn_unused_result [-Wunused-result]
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_parse_a_reply.c -o build/temp.linux-i686-2.7/c-ares/ares_parse_a_reply.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_parse_aaaa_reply.c -o build/temp.linux-i686-2.7/c-ares/ares_parse_aaaa_reply.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_parse_ptr_reply.c -o build/temp.linux-i686-2.7/c-ares/ares_parse_ptr_reply.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_platform.c -o build/temp.linux-i686-2.7/c-ares/ares_platform.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_process.c -o build/temp.linux-i686-2.7/c-ares/ares_process.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_query.c -o build/temp.linux-i686-2.7/c-ares/ares_query.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_search.c -o build/temp.linux-i686-2.7/c-ares/ares_search.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_send.c -o build/temp.linux-i686-2.7/c-ares/ares_send.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_strcasecmp.c -o build/temp.linux-i686-2.7/c-ares/ares_strcasecmp.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_strdup.c -o build/temp.linux-i686-2.7/c-ares/ares_strdup.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_strerror.c -o build/temp.linux-i686-2.7/c-ares/ares_strerror.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_timeout.c -o build/temp.linux-i686-2.7/c-ares/ares_timeout.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_version.c -o build/temp.linux-i686-2.7/c-ares/ares_version.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/ares_writev.c -o build/temp.linux-i686-2.7/c-ares/ares_writev.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/bitncmp.c -o build/temp.linux-i686-2.7/c-ares/bitncmp.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/inet_net_pton.c -o build/temp.linux-i686-2.7/c-ares/inet_net_pton.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/inet_ntop.c -o build/temp.linux-i686-2.7/c-ares/inet_ntop.o
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -DHAVE_CONFIG_H= -DCARES_EMBED=1 -Ibuild/temp.linux-i686-2.7/c-ares -Ic-ares -I/usr/include/python2.7 -c c-ares/windows_port.c -o build/temp.linux-i686-2.7/c-ares/windows_port.o
    gcc -pthread -shared -Wl,-O1 -Wl,-Bsymbolic-functions -Wl,-Bsymbolic-functions -Wl,-z,relro build/temp.linux-i686-2.7/gevent/gevent.ares.o build/temp.linux-i686-2.7/c-ares/ares__close_sockets.o build/temp.linux-i686-2.7/c-ares/ares__get_hostent.o build/temp.linux-i686-2.7/c-ares/ares__read_line.o build/temp.linux-i686-2.7/c-ares/ares__timeval.o build/temp.linux-i686-2.7/c-ares/ares_data.o build/temp.linux-i686-2.7/c-ares/ares_destroy.o build/temp.linux-i686-2.7/c-ares/ares_expand_name.o build/temp.linux-i686-2.7/c-ares/ares_free_hostent.o build/temp.linux-i686-2.7/c-ares/ares_free_string.o build/temp.linux-i686-2.7/c-ares/ares_gethostbyaddr.o build/temp.linux-i686-2.7/c-ares/ares_gethostbyname.o build/temp.linux-i686-2.7/c-ares/ares_getnameinfo.o build/temp.linux-i686-2.7/c-ares/ares_init.o build/temp.linux-i686-2.7/c-ares/ares_library_init.o build/temp.linux-i686-2.7/c-ares/ares_llist.o build/temp.linux-i686-2.7/c-ares/ares_mkquery.o build/temp.linux-i686-2.7/c-ares/ares_nowarn.o build/temp.linux-i686-2.7/c-ares/ares_options.o build/temp.linux-i686-2.7/c-ares/ares_parse_a_reply.o build/temp.linux-i686-2.7/c-ares/ares_parse_aaaa_reply.o build/temp.linux-i686-2.7/c-ares/ares_parse_ptr_reply.o build/temp.linux-i686-2.7/c-ares/ares_platform.o build/temp.linux-i686-2.7/c-ares/ares_process.o build/temp.linux-i686-2.7/c-ares/ares_query.o build/temp.linux-i686-2.7/c-ares/ares_search.o build/temp.linux-i686-2.7/c-ares/ares_send.o build/temp.linux-i686-2.7/c-ares/ares_strcasecmp.o build/temp.linux-i686-2.7/c-ares/ares_strdup.o build/temp.linux-i686-2.7/c-ares/ares_strerror.o build/temp.linux-i686-2.7/c-ares/ares_timeout.o build/temp.linux-i686-2.7/c-ares/ares_version.o build/temp.linux-i686-2.7/c-ares/ares_writev.o build/temp.linux-i686-2.7/c-ares/bitncmp.o build/temp.linux-i686-2.7/c-ares/inet_net_pton.o build/temp.linux-i686-2.7/c-ares/inet_ntop.o build/temp.linux-i686-2.7/c-ares/windows_port.o -lrt -o build/lib.linux-i686-2.7/gevent/ares.so
    Linking /home/brandon/gevent-1.0b4/build/lib.linux-i686-2.7/gevent/ares.so to /home/brandon/gevent-1.0b4/gevent/ares.so
    building 'gevent._semaphore' extension
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -I/usr/include/python2.7 -c gevent/gevent._semaphore.c -o build/temp.linux-i686-2.7/gevent/gevent._semaphore.o
    gcc -pthread -shared -Wl,-O1 -Wl,-Bsymbolic-functions -Wl,-Bsymbolic-functions -Wl,-z,relro build/temp.linux-i686-2.7/gevent/gevent._semaphore.o -o build/lib.linux-i686-2.7/gevent/_semaphore.so
    Linking /home/brandon/gevent-1.0b4/build/lib.linux-i686-2.7/gevent/_semaphore.so to /home/brandon/gevent-1.0b4/gevent/_semaphore.so
    building 'gevent._util' extension
    gcc -pthread -fno-strict-aliasing -DNDEBUG -g -fwrapv -O2 -Wall -Wstrict-prototypes -fPIC -I/usr/include/python2.7 -c gevent/gevent._util.c -o build/temp.linux-i686-2.7/gevent/gevent._util.o
    gcc -pthread -shared -Wl,-O1 -Wl,-Bsymbolic-functions -Wl,-Bsymbolic-functions -Wl,-z,relro build/temp.linux-i686-2.7/gevent/gevent._util.o -o build/lib.linux-i686-2.7/gevent/_util.so
    Linking /home/brandon/gevent-1.0b4/build/lib.linux-i686-2.7/gevent/_util.so to /home/brandon/gevent-1.0b4/gevent/_util.so
    creating build/bdist.linux-i686
    creating build/bdist.linux-i686/egg
    creating build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/threadpool.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/subprocess.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/ssl.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/thread.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/local.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/core.so -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/hub.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/greenlet.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/server.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/resolver_ares.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/timeout.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/queue.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/lock.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/fileobject.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/_semaphore.so -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/_threading.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/wsgi.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/event.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/ares.so -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/pywsgi.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/os.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/win32util.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/resolver_thread.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/select.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/backdoor.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/monkey.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/baseserver.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/util.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/coros.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/pool.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/socket.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/__init__.py -> build/bdist.linux-i686/egg/gevent
    copying build/lib.linux-i686-2.7/gevent/_util.so -> build/bdist.linux-i686/egg/gevent
    byte-compiling build/bdist.linux-i686/egg/gevent/threadpool.py to threadpool.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/subprocess.py to subprocess.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/ssl.py to ssl.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/thread.py to thread.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/local.py to local.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/hub.py to hub.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/greenlet.py to greenlet.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/server.py to server.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/resolver_ares.py to resolver_ares.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/timeout.py to timeout.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/queue.py to queue.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/lock.py to lock.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/fileobject.py to fileobject.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/_threading.py to _threading.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/wsgi.py to wsgi.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/event.py to event.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/pywsgi.py to pywsgi.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/os.py to os.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/win32util.py to win32util.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/resolver_thread.py to resolver_thread.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/select.py to select.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/backdoor.py to backdoor.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/monkey.py to monkey.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/baseserver.py to baseserver.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/util.py to util.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/coros.py to coros.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/pool.py to pool.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/socket.py to socket.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/__init__.py to __init__.pyc
    creating stub loader for gevent/core.so
    creating stub loader for gevent/ares.so
    creating stub loader for gevent/_semaphore.so
    creating stub loader for gevent/_util.so
    byte-compiling build/bdist.linux-i686/egg/gevent/core.py to core.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/ares.py to ares.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/_semaphore.py to _semaphore.pyc
    byte-compiling build/bdist.linux-i686/egg/gevent/_util.py to _util.pyc
    creating build/bdist.linux-i686/egg/EGG-INFO
    copying gevent.egg-info/PKG-INFO -> build/bdist.linux-i686/egg/EGG-INFO
    copying gevent.egg-info/SOURCES.txt -> build/bdist.linux-i686/egg/EGG-INFO
    copying gevent.egg-info/dependency_links.txt -> build/bdist.linux-i686/egg/EGG-INFO
    copying gevent.egg-info/requires.txt -> build/bdist.linux-i686/egg/EGG-INFO
    copying gevent.egg-info/top_level.txt -> build/bdist.linux-i686/egg/EGG-INFO
    writing build/bdist.linux-i686/egg/EGG-INFO/native_libs.txt
    zip_safe flag not set; analyzing archive contents...
    creating dist
    creating 'dist/gevent-1.0dev-py2.7-linux-i686.egg' and adding 'build/bdist.linux-i686/egg' to it
    removing 'build/bdist.linux-i686/egg' (and everything under it)
    Processing gevent-1.0dev-py2.7-linux-i686.egg
    creating /usr/local/lib/python2.7/dist-packages/gevent-1.0dev-py2.7-linux-i686.egg
    Extracting gevent-1.0dev-py2.7-linux-i686.egg to /usr/local/lib/python2.7/dist-packages
    Adding gevent 1.0dev to easy-install.pth file
    
    Installed /usr/local/lib/python2.7/dist-packages/gevent-1.0dev-py2.7-linux-i686.egg
    Processing dependencies for gevent==1.0dev
    Searching for greenlet
    Reading http://pypi.python.org/simple/greenlet/
    Best match: greenlet 0.4.1
    Downloading https://pypi.python.org/packages/source/g/greenlet/greenlet-0.4.1.zip#md5=c2deda75bdda59c38cae12a77cc53adc
    Processing greenlet-0.4.1.zip
    Running greenlet-0.4.1/setup.py -q bdist_egg --dist-dir /tmp/easy_install-6vc6BQ/greenlet-0.4.1/egg-dist-tmp-E8S1bd
    /tmp/easy_install-6vc6BQ/greenlet-0.4.1/temp/tmpBozrVs/simple.c:1:6: warning: function declaration isn’t a prototype [-Wstrict-prototypes]
    greenlet.c: In function ‘g_switch’:
    greenlet.c:593:5: warning: ‘err’ may be used uninitialized in this function [-Wuninitialized]
    Adding greenlet 0.4.1 to easy-install.pth file
    
    Installed /usr/local/lib/python2.7/dist-packages/greenlet-0.4.1-py2.7-linux-i686.egg
    Finished processing dependencies for gevent==1.0dev

### Tried installing Cython 0.19.1 on a Rascal with pip ###

    pip install cython
    Downloading/unpacking cython
      Downloading Cython-0.19.1.tar.gz (1.4Mb): 1.4Mb downloaded
      Running setup.py egg_info for package cython
    Command python setup.py egg_info failed with error code -9 in /home/root/build/cython
    Storing complete log in /home/root/.pip/pip.log

In pip.log:

    Command python setup.py egg_info failed with error code -9 in /home/root/build/cython

    Exception information:
    Traceback (most recent call last):
      File "/usr/lib/python2.6/site-packages/pip/basecommand.py", line 104, in main
        status = self.run(options, args)
      File "/usr/lib/python2.6/site-packages/pip/commands/install.py", line 245, in run
        requirement_set.prepare_files(finder, force_root_egg_info=self.bundle, bundle=self.bundle)
      File "/usr/lib/python2.6/site-packages/pip/req.py", line 1009, in prepare_files
        req_to_install.run_egg_info()
      File "/usr/lib/python2.6/site-packages/pip/req.py", line 225, in run_egg_info
        command_desc='python setup.py egg_info')
      File "/usr/lib/python2.6/site-packages/pip/__init__.py", line 256, in call_subprocess
        % (command_desc, proc.returncode, cwd))
    InstallationError: Command python setup.py egg_info failed with error code -9 in /home/root/build/cython

### Appears that error code -9 means we're running out of memory? ###

Or at least turning on swap helps.

    [root@rascal14:~]: dd if=/dev/zero of=/home/root/swap bs=1M count=100
    100+0 records in
    100+0 records out
    [root@rascal14:~]: chmod 0600 swap
    [root@rascal14:~]: mkswap swap
    Setting up swapspace version 1, size = 104853504 bytes
    [root@rascal14:~]: swapon swap
    [root@rascal14:~]: pip install cython
    Downloading/unpacking cython
    Running setup.py egg_info for package cython
        Compiling module Cython.Compiler.Parsing ...
        Compiling module Cython.Compiler.Visitor ...
        Compiling module Cython.Compiler.FlowControl ...
        Compiling module Cython.Compiler.Code ...
        Compiling module Cython.Runtime.refnanny ...
        warning: no files found matching '*.pyx' under directory 'Cython/Debugger/Tests'
        warning: no files found matching '*.pxd' under directory 'Cython/Debugger/Tests'
        warning: no files found matching '*.h' under directory 'Cython/Debugger/Tests'
        warning: no files found matching '*.pxd' under directory 'Cython/Utility'
    Installing collected packages: cython
      Running setup.py install for cython
        building 'Cython.Plex.Scanners' extension
        arm-angstrom-linux-gnueabi-gcc -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -fexpensive-optimizations -frename-registers -fomit-frame-pointer -O2 -ggdb2 -DNDEBUG -g -O3 -Wall -Wstrict-prototypes -fPIC -I/usr/include/python2.6 -c /home/root/build/cython/Cython/Plex/Scanners.c -o build/temp.linux-armv5tejl-2.6/home/root/build/cython/Cython/Plex/Scanners.o
        /home/root/build/cython/Cython/Plex/Scanners.c: In function '__pyx_pf_6Cython_4Plex_8Scanners_7Scanner_5trace___get__':
        /home/root/build/cython/Cython/Plex/Scanners.c:5246: warning: dereferencing type-punned pointer will break strict-aliasing rules
        /home/root/build/cython/Cython/Plex/Scanners.c:5246: warning: dereferencing type-punned pointer will break strict-aliasing rules
        arm-angstrom-linux-gnueabi-gcc -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -shared build/temp.linux-armv5tejl-2.6/home/root/build/cython/Cython/Plex/Scanners.o -L/usr/lib -lpython2.6 -o build/lib.linux-armv5tejl-2.6/Cython/Plex/Scanners.so
        building 'Cython.Plex.Actions' extension
        arm-angstrom-linux-gnueabi-gcc -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -fexpensive-optimizations -frename-registers -fomit-frame-pointer -O2 -ggdb2 -DNDEBUG -g -O3 -Wall -Wstrict-prototypes -fPIC -I/usr/include/python2.6 -c /home/root/build/cython/Cython/Plex/Actions.c -o build/temp.linux-armv5tejl-2.6/home/root/build/cython/Cython/Plex/Actions.o
        arm-angstrom-linux-gnueabi-gcc -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -shared build/temp.linux-armv5tejl-2.6/home/root/build/cython/Cython/Plex/Actions.o -L/usr/lib -lpython2.6 -o build/lib.linux-armv5tejl-2.6/Cython/Plex/Actions.so
        building 'Cython.Compiler.Lexicon' extension
        arm-angstrom-linux-gnueabi-gcc -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -fexpensive-optimizations -frename-registers -fomit-frame-pointer -O2 -ggdb2 -DNDEBUG -g -O3 -Wall -Wstrict-prototypes -fPIC -I/usr/include/python2.6 -c /home/root/build/cython/Cython/Compiler/Lexicon.c -o build/temp.linux-armv5tejl-2.6/home/root/build/cython/Cython/Compiler/Lexicon.o
        arm-angstrom-linux-gnueabi-gcc -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -shared build/temp.linux-armv5tejl-2.6/home/root/build/cython/Cython/Compiler/Lexicon.o -L/usr/lib -lpython2.6 -o build/lib.linux-armv5tejl-2.6/Cython/Compiler/Lexicon.so
        building 'Cython.Compiler.Scanning' extension
        arm-angstrom-linux-gnueabi-gcc -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -fexpensive-optimizations -frename-registers -fomit-frame-pointer -O2 -ggdb2 -DNDEBUG -g -O3 -Wall -Wstrict-prototypes -fPIC -I/usr/include/python2.6 -c /home/root/build/cython/Cython/Compiler/Scanning.c -o build/temp.linux-armv5tejl-2.6/home/root/build/cython/Cython/Compiler/Scanning.o
        arm-angstrom-linux-gnueabi-gcc -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -shared build/temp.linux-armv5tejl-2.6/home/root/build/cython/Cython/Compiler/Scanning.o -L/usr/lib -lpython2.6 -o build/lib.linux-armv5tejl-2.6/Cython/Compiler/Scanning.so
        building 'Cython.Compiler.Parsing' extension
        arm-angstrom-linux-gnueabi-gcc -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -fexpensive-optimizations -frename-registers -fomit-frame-pointer -O2 -ggdb2 -DNDEBUG -g -O3 -Wall -Wstrict-prototypes -fPIC -I/usr/include/python2.6 -c /home/root/build/cython/Cython/Compiler/Parsing.c -o build/temp.linux-armv5tejl-2.6/home/root/build/cython/Cython/Compiler/Parsing.o
        arm-angstrom-linux-gnueabi-gcc -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -shared build/temp.linux-armv5tejl-2.6/home/root/build/cython/Cython/Compiler/Parsing.o -L/usr/lib -lpython2.6 -o build/lib.linux-armv5tejl-2.6/Cython/Compiler/Parsing.so
        building 'Cython.Compiler.Visitor' extension
        arm-angstrom-linux-gnueabi-gcc -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -fexpensive-optimizations -frename-registers -fomit-frame-pointer -O2 -ggdb2 -DNDEBUG -g -O3 -Wall -Wstrict-prototypes -fPIC -I/usr/include/python2.6 -c /home/root/build/cython/Cython/Compiler/Visitor.c -o build/temp.linux-armv5tejl-2.6/home/root/build/cython/Cython/Compiler/Visitor.o
        /home/root/build/cython/Cython/Compiler/Visitor.c: In function '__pyx_pf_6Cython_8Compiler_7Visitor_2tree_contains':
        /home/root/build/cython/Cython/Compiler/Visitor.c:12162: warning: dereferencing type-punned pointer will break strict-aliasing rules
        /home/root/build/cython/Cython/Compiler/Visitor.c:12162: warning: dereferencing type-punned pointer will break strict-aliasing rules


(Many other type-punned pointer warnings omitted.)

And finally:

    /home/root/build/cython/Cython/Compiler/Code.c:58308: warning: dereferencing type-punned pointer will break strict-aliasing rules
    cc1: out of memory allocating 5419940 bytes after a total of 32534528 bytes
    error: command 'arm-angstrom-linux-gnueabi-gcc' failed with exit status 1

A second attempt with the same swap settings seems to start with where it died, Cython.Compiler.Code, so maybe we can compile this in a few attempts. Stopped Rascal webserver to free up some more RAM.

    [root@rascal14:~]: pip install cython
    Downloading/unpacking cython
      Running setup.py egg_info for package cython
        warning: no files found matching '*.pyx' under directory 'Cython/Debugger/Tests'
        warning: no files found matching '*.pxd' under directory 'Cython/Debugger/Tests'
        warning: no files found matching '*.h' under directory 'Cython/Debugger/Tests'
        warning: no files found matching '*.pxd' under directory 'Cython/Utility'
    Installing collected packages: cython
      Running setup.py install for cython
        building 'Cython.Compiler.Code' extension
        arm-angstrom-linux-gnueabi-gcc -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -fexpensive-optimizations -frename-registers -fomit-frame-pointer -O2 -ggdb2 -DNDEBUG -g -O3 -Wall -Wstrict-prototypes -fPIC -I/usr/include/python2.6 -c /home/root/build/cython/Cython/Compiler/Code.c -o build/temp.linux-armv5tejl-2.6/home/root/build/cython/Cython/Compiler/Code.o

### Trying cross-compilation of Cython instead ###

Built Cython 0.19.1 on Amazon EC2 instance.

    [root@rascal14:~]: opkg install python-cython_0.19.1-r3.6_armv5te.ipk 
    Installing python-cython (0.19.1-r3.6) to root...
    Configuring python-cython.
    [root@rascal14:~]: cython --version  
    Cython version 0.19.1

### Tried building gevent with Cython ###

Missing libevent?

    [root@rascal14:~]: pip install gevent
    Downloading/unpacking gevent
      Downloading gevent-0.13.8.tar.gz (300Kb): 300Kb downloaded
      Running setup.py egg_info for package gevent
    Downloading/unpacking greenlet (from gevent)
      Downloading greenlet-0.4.1.zip (75Kb): 75Kb downloaded
      Running setup.py egg_info for package greenlet
    Installing collected packages: gevent, greenlet
      Running setup.py install for gevent
        building 'gevent.core' extension
        arm-angstrom-linux-gnueabi-gcc -march=armv5te -mtune=arm926ej-s -mthumb-interwork -mno-thumb -fexpensive-optimizations -frename-registers -fomit-frame-pointer -O2 -ggdb2 -DNDEBUG -g -O3 -Wall -Wstrict-prototypes -fPIC -I/usr/include/python2.6 -c gevent/core.c -o build/temp.linux-armv5tejl-2.6/gevent/core.o
        In file included from gevent/core.c:253:
        gevent/libevent.h:9:19: error: event.h: No such file or directory

Followed by lots more spew.

Install libevent.

    [root@rascal14:~]: opkg install libevent
    Unknown package 'libevent'.
    Collected errors:
     * opkg_install_cmd: Cannot install package libevent.
    [root@rascal14:~]: opkg install libevent-1.1a1
    Installing libevent-1.1a1 (1.1a-r0.5) to root...
    Downloading http://rascalmicro.com/packages/beta/armv5te/base/libevent-1.1a1_1.1a-r0.5_armv5te.ipk.
    Configuring libevent-1.1a1.


