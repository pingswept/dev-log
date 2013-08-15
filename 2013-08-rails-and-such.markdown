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
    
    rbenv install 1.9.3-p392
