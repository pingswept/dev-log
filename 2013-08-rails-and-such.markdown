    sudo apt-get install apache2 apache2-mpm-prefork apache2-prefork-dev

    sudo aptitude install ruby build-essential libopenssl-ruby ruby1.8-dev
    
    sudo apt-get install rubygems
    
Check gem version

    gem -v
    
    1.8.15        

Install rails

    sudo gem install rails
    Fetching: i18n-0.6.5.gem (100%)
    Fetching: multi_json-1.7.9.gem (100%)
    Fetching: tzinfo-0.3.37.gem (100%)
    Fetching: minitest-4.7.5.gem (100%)
    Fetching: atomic-1.1.13.gem (100%)
    Building native extensions.  This could take a while...
    Fetching: thread_safe-0.1.2.gem (100%)
    Fetching: activesupport-4.0.0.gem (100%)
    ERROR:  Error installing rails:
    	activesupport requires Ruby version >= 1.9.3.

Seems better to use `rbenv`

    git clone https://github.com/sstephenson/rbenv.git ~/.rbenv

    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc
    echo 'eval "$(rbenv init -)"' >> ~/.zshrc
    
    exec $SHELL -l

    git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
    
    rbenv install 1.9.3-p392 # this version number was wrong in Adam's docs

    rbenv rehash

    rbenv global 1.9.3-p392

Then install `rails`

    gem install rails -v 3.2.12
    rbenv rehash
    bundle install

Create a new app

    rails new crap
    cd crap
    rails server

Error:

    /var/lib/gems/1.8/gems/execjs-1.4.0/lib/execjs/runtimes.rb:51:in `autodetect': Could not find a JavaScript runtime. See https://github.com/sstephenson/execjs for a list of available runtimes. (ExecJS::RuntimeUnavailable)

Install `therubyracer`

    sudo gem install therubyracer

Add to crap/Gemfile:

    gem "therubyracer", :require => 'v8'
    
Works!

    rails server
    => Booting WEBrick
    => Rails 3.2.12 application starting in development on http://0.0.0.0:3000
    => Call with -d to detach
    => Ctrl-C to shutdown server
    [2013-08-15 19:47:58] INFO  WEBrick 1.3.1
    [2013-08-15 19:47:58] INFO  ruby 1.8.7 (2011-06-30) [i686-linux]
    [2013-08-15 19:48:03] INFO  WEBrick::HTTPServer#start: pid=963 port=3000

### Trying to get the real site up ###

    rails server
    Could not find i18n-0.6.1 in any of the sources
    Run `bundle install` to install missing gems.

    sudo bundle install

Installs a bunch of gems, but dies in the middle.

    Installing nokogiri (1.5.6) 
    Gem::Installer::ExtensionBuildError: ERROR: Failed to build gem native extension.
    
            /usr/bin/ruby1.8 extconf.rb 
    checking for libxml/parser.h... yes
    checking for libxslt/xslt.h... no
    -----
    libxslt is missing.  please visit http://nokogiri.org/tutorials/installing_nokogiri.html for help

Nokogiri requires libxslt for installation.

    sudo apt-get install libxslt-dev libxml2-dev
    sudo gem install nokogiri -v '1.5.6'

Bundle install goes further, but still dies.

    Installing factory_girl (4.1.0) 
    Gem::InstallError: factory_girl requires Ruby version >= 1.9.2.
    An error occurred while installing factory_girl (4.1.0), and Bundler cannot
    continue.
    Make sure that `gem install factory_girl -v '4.1.0'` succeeds before bundling.

Problem is that rbenv doesn't know about bundle. Need a bundle shim.

    git clone -- git://github.com/carsomyr/rbenv-bundler.git ~/.rbenv/plugins/bundler
    touch /home/brandon/.rbenv/plugins/bundler/share/rbenv/bundler/manifest.txt
    rbenv rehash
    
    Could not load the bundler gem for Ruby version 1.9.3-p392.

    ruby -r bundler -e "puts RUBY_VERSION"
    /home/brandon/.rbenv/versions/1.9.3-p392/lib/ruby/1.9.1/rubygems/custom_require.rb:36:in `require': cannot load such file -- bundler (LoadError)
    	from /home/brandon/.rbenv/versions/1.9.3-p392/lib/ruby/1.9.1/rubygems/custom_require.rb:36:in `require'
    ruby -v
    ruby 1.9.3p392 (2013-02-22 revision 39386) [i686-linux]
    gem install bundler
    Fetching: bundler-1.3.5.gem (100%)
    Successfully installed bundler-1.3.5
    1 gem installed
    Installing ri documentation for bundler-1.3.5...
    Installing RDoc documentation for bundler-1.3.5...
    ruby -r bundler -e "puts RUBY_VERSION"
    1.9.3
    rbenv rehash
    Bundler gave the error "Could not find rake-10.1.0 in any of the sources" while processing "/home/brandon/rascal-repos/pikauserprofiles/Gemfile". Perhaps you forgot to run "bundle install"?

Then, bundle install

    bundle install
    Fetching gem metadata from https://rubygems.org/.........
    Fetching gem metadata from https://rubygems.org/..
    Installing rake (10.1.0) 
    Installing i18n (0.6.1) 
    Installing multi_json (1.7.7) 
    Installing activesupport (3.2.13) 
    Installing builder (3.0.4) 
    Installing activemodel (3.2.13) 
    Installing erubis (2.7.0) 
    Installing journey (1.0.4) 
    Installing rack (1.4.5) 
    Installing rack-cache (1.2) 
    Installing rack-test (0.6.2) 
    Installing hike (1.2.3) 
    Installing tilt (1.4.1) 
    Installing sprockets (2.2.2) 
    Installing actionpack (3.2.13) 
    Installing mime-types (1.23) 
    Installing polyglot (0.3.3) 
    Installing treetop (1.4.14) 
    Installing mail (2.5.4) 
    Installing actionmailer (3.2.13) 
    Installing arel (3.0.2) 
    Installing tzinfo (0.3.37) 
    Installing activerecord (3.2.13) 
    Installing activeresource (3.2.13) 
    Installing bcrypt-ruby (3.0.1) 
    Installing nokogiri (1.5.6) 
    Installing ffi (1.4.0) 
    Installing childprocess (0.3.9) 
    Installing rubyzip (0.9.9) 
    Installing websocket (1.0.7) 
    Installing selenium-webdriver (2.31.0) 
    Installing xpath (0.1.4) 
    Installing capybara (1.1.2) 
    Installing choice (0.1.6) 
    Installing coffee-script-source (1.6.1) 
    Installing execjs (1.4.0) 
    Installing coffee-script (2.2.0) 
    Installing rack-ssl (1.3.3) 
    Installing json (1.8.0) 
    Installing rdoc (3.12.2) 
    Installing thor (0.18.1) 
    Installing railties (3.2.13) 
    Installing coffee-rails (3.2.2) 
    Installing diff-lcs (1.1.3) 
    Installing gherkin (2.11.6) 
    Installing cucumber (1.2.2) 
    Installing cucumber-rails (1.2.1) 
    Installing database_cleaner (0.7.0) 
    Installing factory_girl (4.1.0) 
    Installing factory_girl_rails (4.1.0) 
    Installing faker (1.0.1) 
    Installing foreigner (1.4.0) 
    Installing jquery-rails (2.2.1) 
    Installing kaminari (0.14.1) 
    Installing mysql2 (0.3.11) 
    Installing polyamorous (0.5.0) 
    Using bundler (1.3.5) 
    Installing rails (3.2.13) 
    Installing ruby-graphviz (1.0.9) 
    Installing rails-erd (1.1.0) 
    Installing ransack (0.7.2) 
    Installing rspec-core (2.11.1) 
    Installing rspec-expectations (2.11.3) 
    Installing rspec-mocks (2.11.3) 
    Installing rspec (2.11.0) 
    Installing rspec-rails (2.11.0) 
    Installing sass (3.2.6) 
    Installing sass-rails (3.2.5) 
    Installing spork (0.9.2) 
    Installing sqlite3 (1.3.7) 
    Installing uglifier (1.2.3) 
    Your bundle is complete!
    Use `bundle show [gemname]` to see where a bundled gem is installed.
    Post-install message from rdoc:
    Depending on your version of ruby, you may need to install ruby rdoc/ri data:
    
    <= 1.8.6 : unsupported
     = 1.8.7 : gem install rdoc-data; rdoc-data --install
     = 1.9.1 : gem install rdoc-data; rdoc-data --install
    >= 1.9.2 : nothing to do! Yay!
    Post-install message from ruby-graphviz:
    
    Since version 0.9.2, Ruby/GraphViz can use Open3.popen3 (or not)
    On Windows, you can install 'win32-open3'
    
    You need to install GraphViz (http://graphviz.org/) to use this Gem.
    
    For more information about Ruby-Graphviz :
    * Doc : http://rdoc.info/projects/glejeune/Ruby-Graphviz
    * Sources : http://github.com/glejeune/Ruby-Graphviz
    * NEW - Mailing List : http://groups.google.com/group/ruby-graphviz
    
    Last (important) changes :
    * GraphViz#add_edge is deprecated, use GraphViz#add_edges
    * GraphViz#add_node is deprecated, use GraphViz#add_nodes
    * GraphViz::Edge#each_attribut is deprecated, use GraphViz::Edge#each_attribute
    * GraphViz::GraphML#attributs is deprecated, use GraphViz::GraphML#attributes
    * GraphViz::Node#each_attribut is deprecated, use GraphViz::Node#each_attribute
      %

Dump the database

    mysqldump -u root -p pikaen_rails > pikaen-railsdb-dump-2013-08-16.sql
    mysql -u root -p
    create database pikaen_rails;
    mysql -u root -p pikaen_rails < pikaen-railsdb-dump-2013-08-16.sql

Get Apache to serve Rails

    sudo apt-get install libcurl4-openssl-dev # for passenger
    gem install passenger
    rbenv rehash
    passenger-install-apache2-module

Add to Apache config file

    LoadModule passenger_module /home/brandon/.rbenv/versions/1.9.3-p392/lib/ruby/gems/1.9.1/gems/passenger-4.0.13/buildout/apache2/mod_passenger.so
    PassengerRoot /home/brandon/.rbenv/versions/1.9.3-p392/lib/ruby/gems/1.9.1/gems/passenger-4.0.13
    PassengerDefaultRuby /home/brandon/.rbenv/versions/1.9.3-p392/bin/ruby

Point DocumentRoot in Apache config to Rails public folder and disable MultiViews

    DocumentRoot /home/brandon/rascal-repos/pikauserprofiles/public
    Options -MultiViews

Precompile Rails assets

    RAILS_ENV=production rake assets:precompile
