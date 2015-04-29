Generate RSA key with ssh-keygen.

Copy `~/.ssh/id_rsa.pub` to `~/.ssh/authorized_keys` on tunnel server.

Put this in `/etc/supervisor/conf.d/autossh-tunnel`

    [program:autossh-tunnel]
    command=autossh -i /root/.ssh/id_rsa -M 12300 -R *:12345:localhost:80 sms@rascalmicro.com -N
    autorestart=true
    startretries=100

`supervisorctl update`

