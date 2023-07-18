```
server {
        listen 80;
        listen [::]:80;

        root /var/www/pingswept.org;
        index index.html index.htm;

        server_name pingswept.org www.pingswept.org;

        location / {
                try_files $uri $uri/ =404;
        }
}

server {
        listen 80;
        listen [::]:80;

        root /var/www/weather.pingswept.org;
        index index.html index.htm;

        server_name weather.pingswept.org;

        location / {
                try_files $uri $uri/ =404;
        }
}

server {
        listen 80;
        listen [::]:80;

        root /var/www/hwtmkstff.com;
        index index.html index.htm;

        server_name hwtmkstff.com www.hwtmkstff.com;

        location / {
                try_files $uri $uri/ =404;
        }
}

server {
        listen 80;
        listen [::]:80;

        root /var/www/rascalmicro.com;
        index index.html index.htm;

        server_name rascalmicro.com www.rascalmicro.com;

        location / { # catch-all directive
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header HOST $http_host;
            proxy_set_header X-NginX-Proxy true;

            proxy_pass http://127.0.0.1:2368;
            proxy_redirect off;
        }

# Redirect / to /index and /blog to /

        location = / { # need this because Ghost can't easily have anything other than blog at root dir
            rewrite .* /index;
            proxy_set_header X-Rewrite-URL $request_uri;
            proxy_pass http://127.0.0.1:2368;
        }

        location = /blog/ {
            rewrite .* /;
            proxy_set_header X-Rewrite-URL $request_uri;
            proxy_pass http://127.0.0.1:2368;
            break; # prevents redirection back to index via /
        }

        location /blog/ {                     # redirect old links to blog subdir
            rewrite ^/blog(.*)$ $1 permanent; # snip out the subdir
        }

# Redirect old links that ended in .html

        location ~ .\.html? {                   # redirect old links to .htm or .html
            rewrite ^(.*)\.html?$ $1 permanent; # chop off the extension
        }

# Redirect previous docs directory to docs- prefix
# because Ghost can't handle subdirectories

        location = /docs/ {
            proxy_pass http://127.0.0.1:2368;
        }

        location /docs/ {
            rewrite ^(/docs)/(.*)$ $1-$2;
            proxy_set_header X-Rewrite-URL $request_uri;
            proxy_pass http://127.0.0.1:2368;
        }

# Various static files

        location /img/ {
            root /var/www/rascalmicro.com;
        }

        location /files/ {
            root /var/www/rascalmicro.com;
            autoindex on;
        }

        location /packages/ {
            root /var/www/rascalmicro.com;
            autoindex on;
        }

        location /sources/ {
            root /var/www/rascalmicro.com;
            autoindex on;
        }

        location /robots.txt {
            root /var/www/rascalmicro.com;
        }
# Redirect a few odd URLs with dots and commas
# because those are illegal in Ghost slugs

        location /blog/2012/04/26/open-source-hardware-in-washington,-d.c./ {
            return 301 /2012/04/26/open-source-hardware-in-washington-dc/;
        }

        location /blog/2010/11/01/rascal-0.4-with-arduinos-and-shields/ {
            return 301 /2010/11/01/rascal-0-4-with-arduinos-and-shields/;
        }

        location /blog/2010/09/28/rascal-0.3-in-the-works/ {
            return 301 /2010/09/28/rascal-0-3-in-the-works/;
        }

        location /blog/2010/08/24/rascal-0.2:-launching-rockets-at-the-sky/ {
            return 301 /2010/08/24/rascal-0-2-launching-rockets-at-the-sky/;
        }

        location /blog/2012/03/06/the-first-rascal-1.1-prototypes-have-arrived/ {
            return 301 /2012/03/06/the-first-rascal-1-1-prototypes-have-arrived/;
        }

        location /blog/2012/01/31/rascal-1.1-circuit-board-released-for-fabrication/ {
            return 301 /2012/01/31/rascal-1-1-circuit-board-released-for-fabrication/;
        }

# Redirect old RSS and Atom feeds

        location /blog/feed/index.xml {
            return 301 /rss/;
        }

        location /blog/feed/atom/index.xml {
            return 301 /rss/;
        }
}
```
