### Building fadecandy server ###

    git clone https://github.com/scanlime/fadecandy.git
    root@rascal-lights:~/fadecandy# cd fadecandy/server
    root@rascal-lights:~/fadecandy/server# make submodules
    cd .. && git submodule update --init -- server/libusbx && cd server/libusbx && git checkout -f HEAD && git clean -dfx
    Submodule 'server/libusbx' (https://github.com/scanlime/libusbx.git) registered for path 'server/libusbx'
    Cloning into 'server/libusbx'...
    remote: Counting objects: 9781, done.
    remote: Compressing objects: 100% (3339/3339), done.
    remote: Total 9781 (delta 6093), reused 9781 (delta 6093)
    Receiving objects: 100% (9781/9781), 4.80 MiB | 1.04 MiB/s, done.
    Resolving deltas: 100% (6093/6093), done.
    Checking connectivity... done.
    Submodule path 'server/libusbx': checked out '6899a2647f9cff79d839587a0b81780bdbb13b40'
    cd .. && git submodule update --init -- server/libwebsockets && cd server/libwebsockets && git checkout -f HEAD && git clean -dfx
    Submodule 'server/libwebsockets' (https://github.com/scanlime/libwebsockets.git) registered for path 'server/libwebsockets'
    Cloning into 'server/libwebsockets'...
    remote: Counting objects: 4107, done.
    remote: Compressing objects: 100% (1478/1478), done.
    remote: Total 4107 (delta 2608), reused 4107 (delta 2608)
    Receiving objects: 100% (4107/4107), 5.59 MiB | 1.37 MiB/s, done.
    Resolving deltas: 100% (2608/2608), done.
    Checking connectivity... done.
    Submodule path 'server/libwebsockets': checked out '8028bdf25c2780ec4d931b195fea70e4e8269125'
    cd .. && git submodule update --init -- server/rapidjson && cd server/rapidjson && git checkout -f HEAD && git clean -dfx
    Submodule 'server/rapidjson' (https://github.com/scanlime/rapidjson.git) registered for path 'server/rapidjson'
    Cloning into 'server/rapidjson'...
    remote: Counting objects: 24, done.
    remote: Compressing objects: 100% (19/19), done.
    remote: Total 24 (delta 5), reused 24 (delta 5)
    Unpacking objects: 100% (24/24), done.
    Checking connectivity... done.
    Submodule path 'server/rapidjson': checked out '0c69df5ac098640018d9232ae71ed1036c692187'
