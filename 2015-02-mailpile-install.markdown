
    wget https://github.com/mailpile/Mailpile/archive/0.4.2.tar.gz
    tar xzvf 0.4.2.tar.gz
    cd Mailpile-0.4.2
    
    sudo apt-get install gnupg openssl
    gnupg is already the newest version.
    openssl is already the newest version.
    
    sudo pip install -r requirements.txt
    
    ./mp
    
    mailpile> setup
    http://0.0.0.0:33411 "GET / HTTP/1.1" 200 633
    http://0.0.0.0:33411 "GET /favicon.ico HTTP/1.1" 404 -
    http://0.0.0.0:33411 "GET /favicon.ico HTTP/1.1" 404 -
    Elapsed: 0.000s (000: Opening: /home/brandon/.mailpile/mail (may take a while))
    Succeeded: Perform initial setup

