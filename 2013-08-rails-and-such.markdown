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

Then install `rails`

    sudo gem install rails -v 3.2.12

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
    

