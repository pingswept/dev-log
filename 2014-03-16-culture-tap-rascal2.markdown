Notes on boot times

Boot: around 10 seconds, but takes more like 30 seconds for Avahi broadcast so .local address resolves.
Shutdown: around 15 seconds.

### Install supervisor ###

Supervisor is a Python program that runs other programs as subprocesses: http://supervisord.org/introduction.html

    pacman -S supervisor   
    resolving dependencies...
    looking for inter-conflicts...

    Packages (2): python2-meld3-0.6.10-2  supervisor-3.0-1

Start with `systemctl`. Note that the service unit is called supervisord, not just supervisor.

    systemctl start supervisord                      
    ➜  ~  systemctl | grep super     
    supervisord.service    loaded active running    Process Monitoring and Control Daemon

Enabling on boot under `systemd`

    ➜  ~  systemctl is-enabled supervisord
    disabled
    ➜  ~  systemctl enable supervisord
    ln -s '/usr/lib/systemd/system/supervisord.service' '/etc/systemd/system/multi-user.target.wants/supervisord.service'
    ➜  ~  systemctl is-enabled supervisord
    enabled

