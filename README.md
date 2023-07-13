# j2e_docker

## History of Docker :
- build in 1970
- ldom's in AIX
- Zones in Solaris
- LXC in Linux
- Docker open source

## Links 
- Remote docker : https://docs.docker.com/config/daemon/remote-access/

## commands :
- `docker pull nginx:1.25` - Docker pull a specific version.
-  `docker pull nginx` - pull latest image
-  `docker run nginx:1.25` - Run the docker images
-  `docker run nginx:1.25.1-alpine` - pulls and runs the image
-  `docker images`Check the sizes of normal vs alpine sizes 
-  `docker ps` - check the running containers
-  `docker ps -a` - check the all the containers which are stopped and running
-  `31f79e3b24ea016426039c58deb30e01ff3f4f87920607f7ea95abb951a8cd85` - 65 characters - `31f79e3b24ea` - 13 characters
-  `docker stop <Container id>` - Stop the running containers
- `docker run -d -p 8089:80 nginx:1.25.1-alpine` - Docker run with detached mode and expose port 80 port to the host port 8089
- `cat /etc/services` - List of system used ports
-  `docker logs <Container id>` - get the logs of container, Check the logs for mac ip info.
-  
