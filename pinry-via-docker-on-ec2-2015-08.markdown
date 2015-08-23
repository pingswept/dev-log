Start EC2 instance Ubuntu 14.04 LTS 64-bit t2.micro.

    sudo apt-get update
    sudo apt-get install docker.io

    service docker.io status
    docker.io start/running, process 2651

    git clone https://github.com/pinry/docker-pinry
    cd docker-pinry
    
Enable signups for new users by editing ``pinry/settings/__init__.py``.

    ALLOW_NEW_REGISTRATIONS = True

Hide from the public.

    PUBLIC = False

Build Pinry with Docker

    sudo docker build -t pinry/pinry .

Make directory

    sudo mkdir -p /mnt/pinry
    sudo: unable to resolve host ip-172-30-0-249 # still worked, even though this looks like an error

Start Pinry

    sudo docker run -d=true -p=80:80 -v=/mnt/pinry:/data pinry/pinry /start
    sudo: unable to resolve host ip-172-30-0-249
    9b976361ac1a0c8ce95d2d27e750f2a2bb7274592c987ec032d3b7589f94e244
