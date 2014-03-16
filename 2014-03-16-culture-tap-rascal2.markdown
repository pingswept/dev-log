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
