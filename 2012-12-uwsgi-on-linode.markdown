### uWSGI on Linode ###

uwsgi --master --http 0.0.0.0:8080 --plugin python --file /var/www/ip-app.py --callable app
