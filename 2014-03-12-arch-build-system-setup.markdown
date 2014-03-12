### Overall plan ###

  # Install ABS
  # Sync ABS tree
  # Copy PKGBUILDs to directory
  # `makepkg` for all PKGBUILDS
  # Use repo-add to generate repo database
  # Upload repo database and .pkg.tar.xz files to repo server

    ➜  ~  pacman -S abs
    resolving dependencies...
    looking for inter-conflicts...
    
    Packages (2): rsync-3.1.0-1  abs-2.4.4-1
    
    Total Download Size:    0.23 MiB
    Total Installed Size:   0.58 MiB
    
    :: Proceed with installation? [Y/n] Y
    :: Retrieving packages ...
     rsync-3.1.0-1-armv7h                                                             224.3 KiB  1304K/s 00:00 [################################################################] 100%
     abs-2.4.4-1-armv7h                                                                 7.9 KiB  3.86M/s 00:00 [################################################################] 100%
    (2/2) checking keys in keyring                                                                             [################################################################] 100%
    (2/2) checking package integrity                                                                           [################################################################] 100%
    (2/2) loading package files                                                                                [################################################################] 100%
    (2/2) checking for file conflicts                                                                          [################################################################] 100%
    (2/2) checking available disk space                                                                        [################################################################] 100%
    (1/2) installing rsync                                                                                     [################################################################] 100%
    (2/2) installing abs                                                                                       [################################################################] 100%
    ➜  ~  abs
    ==> Downloading tarballs...
        ==> core...
        ==> extra...
        ==> community...
        ==> multilib...
    ==> ERROR: Download failed
