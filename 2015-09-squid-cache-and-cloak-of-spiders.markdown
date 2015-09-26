### Installing Squid on EC2 ###

Start an Ubuntu t1.micro instance on EC2; SSH into console.

    sudo apt-get update
    sudo apt-get install squid
    sudo service squid3 status
    sudo cp /etc/squid3/squid.conf /etc/squid3/squid.conf.default
    sudo vim /etc/squid3/squid.conf

Add to `/etc/squid3/squid.conf`

    acl name-of-location src 12.34.56.78 # put in location name and browser machine's public IP address
    http_access allow name-of-location
    forwarded_for off
    request_header_access Allow allow all
    request_header_access Authorization allow all
    request_header_access WWW-Authenticate allow all
    request_header_access Proxy-Authorization allow all
    request_header_access Proxy-Authenticate allow all
    request_header_access Cache-Control allow all
    request_header_access Content-Encoding allow all
    request_header_access Content-Length allow all
    request_header_access Content-Type allow all
    request_header_access Date allow all
    request_header_access Expires allow all
    request_header_access Host allow all
    request_header_access If-Modified-Since allow all
    request_header_access Last-Modified allow all
    request_header_access Location allow all
    request_header_access Pragma allow all
    request_header_access Accept allow all
    request_header_access Accept-Charset allow all
    request_header_access Accept-Encoding allow all
    request_header_access Accept-Language allow all
    request_header_access Content-Language allow all
    request_header_access Mime-Version allow all
    request_header_access Retry-After allow all
    request_header_access Title allow all
    request_header_access Connection allow all
    request_header_access Proxy-Connection allow all
    request_header_access User-Agent allow all
    request_header_access Cookie allow all
    request_header_access All deny all

Then

    sudo service squid3 restart

Set browser or system proxy to public IP of EC2 instance, port 3128 for HTTP and HTTPS.

Add inbound firewall rule for EC2 instance allowing connections to port 3128 from browser machine's IP.
