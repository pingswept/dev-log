    sudo passwd

    ubuntu@ubuntu-standard:~$ sudo /snap/bin/nextcloud.enable-https
    In order for Let's Encrypt to verify that you actually own the domain for
    which you're requesting a certificate, there are a number of requirements
    of which you need to be aware:

    1. In order to register with the Let's Encrypt ACME server, you must agree
       to the currently-in-effect Subscriber Agreement located here:

           https://letsencrypt.org/repository/

       By continuing to use this tool you agree to these terms. Please cancel
       now if otherwise.

    2. You must have the domain name(s) for which you want certificates
       pointing at the external IP address of this machine.

    3. Both ports 80 and 443 on the external IP address of this machine must
       point to this machine (e.g. port forwarding might need to be setup on
       your router).

    Have you met these requirements? (y/n) y
    Please enter an email address (for urgent notices or key recovery): brandon@pingswept.org
    Please enter your domain name(s) (space-separated): snowden.pingswept.org
    Attempting to obtain certificates... done
    Restarting apache... done


    sudo vim /var/snap/nextcloud/173/nextcloud/config/config.php
