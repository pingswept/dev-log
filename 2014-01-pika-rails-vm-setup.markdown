### Create an Ubuntu VM on Amazon EC2 ###

1. Log into Amazon Web Services web console: https://console.aws.amazon.com/console/home

2. Click on EC2 (Elastic Compute Cloud)

3. Click "Launch Instance" and select Ubuntu Server 12.04.3 LTS - ami-a73264ce (64-bit). LTS is the long-term support release of Ubuntu, so this version should be supported until 2017.

4. Click "Review and Launch" then "Launch." You'll have to set up a key pair along the way. It's convenient to store your key pair in your Dropbox.

5. From the AWS console page, get the "Public DNS" info for your instance.

6. ssh -i Dropbox/path/to/your-key.pem ubuntu@ec2-54-196-172-74.compute-1.amazonaws.com

7. Accept the new RSA key fingerprint.

8. You're in!

Port 22 is open by default, but you'll want to open two other ports: 80 for Apache and 3000 for the Rails test server.

Back in the AWS web console, click on Network & Security > Security Groups and select your group (probably launch-wizard N, recently created). On the Inbound tab, apply two new rules: Custom TCP rule, port 80, source 0.0.0.0/0 and then the same for port 3000.

### Install some general Linux tools ###

	sudo apt-get update # update repository lists
	sudo apt-get install build-essential git zsh
	sudo -s
	chsh -s /bin/zsh ubuntu # change the shell to Zsh for the default user, ubuntu
	exit
	curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh

### Install rbenv and the current version of Ruby ###

Rbenv is a program for installing and maintaining specific versions of Ruby and Rails. More details here: https://github.com/sstephenson/rbenv

	git clone https://github.com/sstephenson/rbenv.git ~/.rbenv

	echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc
	echo 'eval "$(rbenv init -)"' >> ~/.zshrc

	exec $SHELL -l

	git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build

	rbenv install 1.9.3-p392 # this version number was wrong in Adam's docs

	rbenv rehash # This updates Rbenv's shims, which are links to specific versions of Ruby, Rails, etc.

	ls /home/ubuntu/.rbenv/shims # You can list the shims like this

	rbenv global 1.9.3-p392

### Install MySQL ###

Install MySQL 

For Ubuntu:

	sudo apt-get install mysql-server

For Ubuntu, you'll be prompted (twice) for a root password.

For OS X, install both the main package and the MySQLStartupItem.pkg from http://dev.mysql.com/downloads/mysql/5.1.html#macosx-dmg

Log in to the MysQL console.

    mysql -u root -p

For OS X, add a root password.

    USE mysql;
    UPDATE user set password=PASSWORD("this-is-the-password") WHERE User='root';
    FLUSH PRIVILEGES;

Delete anonymous accounts.

	USE mysql;
    SELECT host, user, password FROM user;
    DROP USER ''@'localhost';
    DROP USER ''@'fosco.tyler'; # this will be something like ip-10-9-131-50 for an Amazon VM

Add pikaen_rails user.

    CREATE USER 'pikaen_rails'@'localhost' IDENTIFIED BY 'stick-the-password-here';
    GRANT ALL PRIVILEGES ON pikaen_rails.* TO 'pikaen_rails'@'localhost';
    FLUSH PRIVILEGES;

Exit the MySQL console.

	exit

### Install Rails ###

	sudo apt-get install rubygems
	sudo apt-get install libsqlite3-dev libxslt-dev libxml2-dev libcurl4-openssl-dev

Libsqlite3-dev is needed for the default Rails install, even though we're not using SQLite. Libxslt-dev and libxml2-dev are needed to build the gem for the Nokogiri XML parser later on. Libcurl4-openssl-dev is for Passenger, the Ruby app server for Apache.

Add the line below to /home/ubuntu/.gemrc (speeds up gem installs by skipping documentation installation)

	gem: --no-document

Install Rails.

    sudo gem install rails -v 3.2.12
    rbenv rehash
    sudo gem install bundler
    bundle install # will fail if MySQL is not installed

Install the Ruby Racer Javascript engine.

    sudo gem install therubyracer

Test Rails by making a new app.

	rails new test-app

Add to test-app/Gemfile:

    gem "therubyracer", :require => 'v8'

Run the Rails development server.

    cd test-app
    rails server

If it works, you'll see something like this:

	=> Booting WEBrick
	=> Rails 3.2.12 application starting in development on http://0.0.0.0:3000
	=> Call with -d to detach
	=> Ctrl-C to shutdown server
	[2014-01-14 17:18:54] INFO  WEBrick 1.3.1
	[2014-01-14 17:18:54] INFO  ruby 1.8.7 (2011-06-30) [x86_64-linux]
	[2014-01-14 17:18:54] INFO  WEBrick::HTTPServer#start: pid=1720 port=3000

You can browse to the IP address of the VM and the port of and see the Rails welcome page. The address should be something like:

	ec2-54-196-172-74.compute-1.amazonaws.com:3000

Once it works, Ctrl-C to quit the Rails server and delete the test-app folder.

### Install the Pika Rails app ###

	git clone <URL of Bitbucket repo>
	cd pikauserprofiles
	bundle install
	rails server

### Load a database dump ###

On the production server:

    mysqldump -u root -p pikaen_rails > pikaen-railsdb-dump-2013-08-16.sql

Copy the SQL file from production to development (might want to use tar for compression along the way).

Then on the development server:

    mysql -u root -p
    create database pikaen_rails;
    mysql -u root -p pikaen_rails < pikaen-railsdb-dump-2013-08-16.sql

### Install and configure Apache ###

Get Apache to serve Rails

    sudo gem install passenger
    rbenv rehash
    passenger-install-apache2-module

Add to Apache config file

    LoadModule passenger_module /home/brandon/.rbenv/versions/1.9.3-p392/lib/ruby/gems/1.9.1/gems/passenger-4.0.13/buildout/apache2/mod_passenger.so
    PassengerRoot /home/brandon/.rbenv/versions/1.9.3-p392/lib/ruby/gems/1.9.1/gems/passenger-4.0.13
    PassengerDefaultRuby /home/brandon/.rbenv/versions/1.9.3-p392/bin/ruby

Point DocumentRoot in Apache config to Rails public folder and disable MultiViews

    DocumentRoot /home/ubuntu/pikauserprofiles/public
    Options -MultiViews

Precompile Rails assets

    RAILS_ENV=production rake assets:precompile
