On Mac OS X using Docker Machine

docker-machine start default
docker-machine env default
eval "$(docker-machine env default)"

clone repo to directory
cd into directory
docker-compose up -d

Get Docker Machine IP:
docker-machine ip default

browse to Docker Machine IP:
http://192.168.99.100
http://192.168.99.100/phpinfo.php

