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

### Installing not using Bitnami ###

Note: the rest of this was partially updated to use new releases of Node and Ruby, but then I realized that I'd rather use the stock Ubuntu distributions of Ruby if I'm not using Rails/Discourse, as I think is now the case. For updated Ghost build/install, see https://github.com/pingswept/dev-log/blob/master/2014-01-ghost-and-node-install.markdown for a newer guide for just Node and Ghost.

Installing on an Amazon EC2 t1.micro instance running Ubuntu Server 12.04.3 LTS - ami-a73264ce (64-bit)

### General EC2 Ubuntu fixups ###

    sudo apt-get update
    sudo apt-get install build-essential git zsh
    sudo -s
    chsh -s /bin/zsh ubuntu # change the shell to Zsh for the default user, ubuntu

Log out, then back in again.

    curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh
    curl -o ~/.vimrc https://raw.github.com/pingswept/linux-tool-configs/master/vimrc

Also, copy gitconfig from https://github.com/pingswept/linux-tool-configs

### Install Varnish ###

Varnish has a package repo for the Ubuntu LTS releases.

    curl http://repo.varnish-cache.org/debian/GPG-key.txt | sudo apt-key add -
    echo "deb http://repo.varnish-cache.org/ubuntu/ precise varnish-3.0" | sudo tee -a /etc/apt/sources.list
    sudo apt-get update
    sudo apt-get install varnish

Update /etc/varnish/default.vcl

    backend default {
        .host = "127.0.0.1";
        .port = "2368"; # for Ghost running on node.js
    }

Update /etc/defaults/varnish

    DAEMON_OPTS="-a :80 \
                 -T localhost:6082 \
                 -f /etc/varnish/default.vcl \
                 -S /etc/varnish/secret \                                                                     
                 -s malloc,256m"

The current release of Ghost doesn't handle cookies properly, which means Varnish breaks authentication, and CSS and JS are marked as not cacheable, which slows stuff down. This stuff will supposedly be fixed in 0.4, but here are some workarounds, at least for the cookies: http://we.je/using-varnish-not-nginx-to-run-ghost/

### Install Rails 4 ###

    gem install rails -v 4.0.1
    rbenv rehash

### Install Postgres ###

Postgres is needed for Discourse. The postrgresql-contrib package is needed for the hstore extension that Discourse uses.

    sudo apt-get install postgresql postgresql-contrib
    sudo -u postgres createuser ubuntu -s -P # not sure this was necessary, maybe just needed to set password
    sudo -u postgres createdb dtest
    sudo -u postgres psql -c "grant all privileges on database dtest to ubuntu"
    sudo -u postgres psql dtest -c "create extension hstore; create extension pg_trgm"

### Install Discourse ###

    gem install bundler
    wget https://github.com/discourse/discourse/archive/v0.9.7.6.tar.gz
    tar xzf v0.9.7.6.tar.gz
    cd discourse-0.9.7.6
    bundle install
    
    OpenSSL::SSL::SSLError: SSL_connect returned=1 errno=0 state=SSLv3 read server hello A: wrong version number
    An error occurred while installing listen (0.7.3), and Bundler cannot continue.
    Make sure that `gem install listen -v '0.7.3'` succeeds before bundling.

Hmmmm.
