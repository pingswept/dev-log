uwsgi --http 0.0.0.0:5000 --ini public.ini

uwsgi --https :8443,foobar.crt,foobar.key --http-raw-body --gevent 100 --module public --callable public
