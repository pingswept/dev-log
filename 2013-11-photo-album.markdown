Add to nginx.conf

    client_max_body_size 10M;

Change in rascal-1.03.js

    maxFileBytes: 1024 * 1024,

Uncomment and adjust in server.py

    # public.config['MAX_CONTENT_LENGTH'] = 4 * 1024 * 1024
