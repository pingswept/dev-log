### Adding swap file ###

```sh
[root@rascal14:~]: dd if=/dev/zero of=/home/root/swap bs=1M count=100        
100+0 records in
100+0 records out
[root@rascal14:~]: chmod 0600 swap
[root@rascal14:~]: mkswap swap
Setting up swapspace version 1, size = 102396 KiB
no label, UUID=4b17557d-afce-4e73-aea7-5a124e92355b
[root@rascal14:~]: swapon swap
```

Useful refs:

* http://www.linuxquestions.org/questions/linux-general-1/2-6-3-swapon-function-not-implemented-155442/
* http://askubuntu.com/questions/126018/adding-a-new-swap-file-how-to-edit-fstab-to-enable-swap-after-reboot/126049#126049
