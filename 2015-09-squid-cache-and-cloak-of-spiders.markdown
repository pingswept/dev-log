### Installing Squid on EC2 ###

Start an Ubuntu t1.micro instance on EC2; SSH into console.

    sudo apt-get update
    sudo apt-get install squid
    sudo service squid3 status
    sudo cp /etc/squid3/squid.conf /etc/squid3/squid.conf.default
    sudo vim /etc/squid3/squid.conf

Add to `/etc/squid3/squid.conf`

    acl name-of-location src 12.34.56.78 # put in location name and public IP address
    http_access allow name-of-location

Then

    sudo service squid3 restart
