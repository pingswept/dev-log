
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

Deleted old .mailpile folder

    mailpile> setup
    mailbox = {'000': '/home/brandon/.mailpile/mail'}                              
    local_mailbox = 000
    tag = {'0': 'New'}                                                             
    tag = {'1': 'Inbox', '0': 'New'}
    tag = {'1': 'Inbox', '0': 'New', '2': 'Spam'}
    tag = {'1': 'Inbox', '0': 'New', '3': 'Drafts', '2': 'Spam'}
    tag = {'1': 'Inbox', '0': 'New', '3': 'Drafts', '2': 'Spam', '4': 'Sent'}
    tag = {'1': 'Inbox', '0': 'New', '3': 'Drafts', '2': 'Spam', '5': 'Trash', '4': 'Sent'}
    filter = {'0': 'New Mail filter'}
    filter_tags = {'0': '+1 +0'}
    filter_terms = {'0': '*'}
    filter = {'1': 'Read Mail filter', '0': 'New Mail filter'}                     
    filter_tags = {'1': '-0', '0': '+1 +0'}
    filter_terms = {'1': '@read', '0': '*'}
    Succeeded: Perform initial setup
