uwsgi --http 0.0.0.0:5000 --ini public.ini

uwsgi --https :8443,foobar.crt,foobar.key --http-raw-body --gevent 100 --module public --callable public

uwsgi --http 0.0.0.0:5000 --static-map /favicon.ico=/var/www/public/static/images/led.gif --ini public.ini

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
