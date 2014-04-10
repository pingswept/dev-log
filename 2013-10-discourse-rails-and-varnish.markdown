### Testing with the Bitnami Discourse AMI ###

Docs: http://wiki.bitnami.com/Applications/BitNami_Discourse

Update SMTP settings in apps/discourse/htdocs/config/environments/production.rb

    config.action_mailer.delivery_method = :smtp
    config.action_mailer.smtp_settings = {
      :address              => "smtp.mandrillapp.com",
      :port                 => 587,
      :domain               => 'rascalmicro.com',
      :user_name            => 'mandrill-user-name',
      :password             => 'stick the Mandrill API key here',
      :authentication       => 'login',
      :enable_starttls_auto => true  }

Restart server after config changes

    sudo /opt/bitnami/ctlscript.sh restart

Changing domain name

    sudo /opt/bitnami/apps/discourse/updateip --machine_hostname projects.pingswept.org
    sudo /opt/bitnami/ctlscript.sh restart
    sudo /opt/bitnami/apps/discourse/scripts/rebakeposts.sh # regenerates posts with new domain used in refs

### Installing Discourse using Docker ###

This is now the supported method for Ubuntu.

Made a fresh Linode 1024 install of 64-bit Ubuntu 12.04 LTS

The [Docker install instructions][1] say you should upgrade to the 3.8 kernel like this:

    sudo apt-get install linux-image-generic-lts-raring linux-headers-generic-lts-raring

This command appeared to succeed, but upon rebooting, it appears that I'm running the 3.13 kernel:

    uname -a
    Linux li134-36 3.13.7-x86_64-linode38 #1 SMP Tue Mar 25 12:59:48 EDT 2014 x86_64 x86_64 x86_64 GNU/Linux

Not sure if that matters or not.

Ah, it does. Have to run stock Ubuntu kernel to get AUFS support. Follow instructions here: https://library.linode.com/custom-instances/pv-grub-howto#sph_ubuntu-12-04-precise

Then, we get:

    uname -a
    Linux li134-36 3.8.0-38-generic #56~precise1-Ubuntu SMP Thu Mar 13 16:22:48 UTC 2014 x86_64 x86_64 x86_64 GNU/Linux

Then HTTPS transport

    sudo apt-get install apt-transport-https

Add Docker repo

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9
    sudo sh -c "echo deb https://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list"
    sudo apt-get update
    sudo apt-get install lxc-docker

Test that Docker works

    sudo docker run -i -t ubuntu /bin/bash

Make directory /var/docker and set its permissions.

    sudo install -g docker -m 2775 -d /var/docker

Add my account to the docker group.

    sudo usermod -a -G docker bstafford

Then actually install Discourse

    git clone https://github.com/discourse/discourse_docker.git /var/docker
    cd /var/docker
    cp samples/standalone.yml containers/app.yml

Edit `containers/app.yml`

Add the Mandrill credentials and set the hostname.

    sudo ./launcher bootstrap app
    sudo ./launcher start app

[1]: http://docs.docker.io/en/latest/installation/ubuntulinux/
