The contents of /etc/init.d/S02network

    #!/bin/sh
    #
    # Start the network....
    #
    
    MACADDR=null
    IPADDR=null
    NETMASK=null
    
    if [ -e "/etc/network/interfaces.eth0" ]; then
        MACADDR=`awk '/MACADDRESS/ {print $1}' /etc/network/interfaces.eth0 |cut -d '=' -f 2`
        IPADDR=`awk '/IPADDRESS/ {print $1}' /etc/network/interfaces.eth0 |cut -d '=' -f 2`
        NETMASK=`awk '/NETMASK/ {print $1}' /etc/network/interfaces.eth0 |cut -d '=' -f 2`
    fi
    
    # start lo
    /sbin/ifconfig lo up
    
    case "$1" in
      start)
        echo "Starting network..."
    #   /sbin/ifup -a
    
        cat /proc/cmdline |grep "nfsroot" > /dev/null
        if [ $? = 0 ]; then
            exit 1
        fi
    
        if ! [ $MACADDR = null ]; then
            /sbin/ifconfig eth0 down
            /sbin/ifconfig eth0 hw ether $MACADDR
        fi
        if ! [ $IPADDR = null ] && ! [ $NETMASK = null ]; then
            /sbin/ifconfig eth0 $IPADDR netmask $NETMASK
        fi
        #/sbin/ifconfig eth0 192.192.192.211 netmask 255.255.255.0
        /sbin/ifconfig eth0 up
        ;;
      stop)
        echo -n "Stopping network..."
    #   /sbin/ifdown -a
    #   /sbin/ifconfig eth0 down
    #   /sbin/ifconfig eth1 down
        ;;
      restart|reload)
        "$0" stop
        "$0" start
        ;;
      *)
        echo $"Usage: $0 {start|stop|restart}"
        exit 1
    esac
    
    exit $?
