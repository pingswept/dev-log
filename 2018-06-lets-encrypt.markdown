I have a few different SSL certificates that use Let's Encrypt. These are the details on how to renew the certificates when they expire.

### Mail server ###

* Make sure that port 80 is port-forwarded to mail server.
* SSH into mail server
* Run `sudo /home/ubuntu/mailinabox/management/ssl_certificates.py`

If there is an error like this: `[NeedToInstallFile('http://mail.pingswept.org/.well-known/acme-challenge/3CB2Cj0UnMJO9WPiAtvn6XIh-liVyyUsLzZBcc', '3CB2Cj0UnMJO9WPiAtvn6XIh-liVyyUsLzZBcc.du8hQnFzPsGOph3aMdyrq2rzTrv4vno-iY6o', '3CB2Cj0UnMJO9WPiAtvn6XIh-liVyyUsLzZBcc')]` then make a file called: `/home/user-data/www/default/.well-known/acme-challenge/3CB2Cj0UnMJO9WPiAtvn6XIh-liVyyUsLzZBcc` that contains `3CB2Cj0UnMJO9WPiAtvn6XIh-liVyyUsLzZBcc.du8hQnFzPsGOph3aMdyrq2rzTrv4vno-iY6o`

Then run `ssl_certificates.py` again.
