# j2e_docker

docker pull nginx:1.25 
docker pull nginx:1.25.1-alpine
docker pull jenkins
docker run -p 8080:8080 -p 50000:50000 -v /your/home:/var/jenkins_home jenkins

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
-  `docker search nginx` - Search for docker images
-  `docker ps` - check the running containers
-  `docker ps -a` - check the all the containers which are stopped and running
-  `31f79e3b24ea016426039c58deb30e01ff3f4f87920607f7ea95abb951a8cd85` - 65 characters - `31f79e3b24ea` - 13 characters
-  `docker stop <Container id>` - Stop the running containers
-  `docker start 85fe326b44d6` - Start the running container
- `docker run -d -p 8089:80 nginx:1.25.1-alpine` - Docker run with detached mode and expose port 80 port to the host port 8089
- `cat /etc/services` - List of system used ports
-  `docker logs <Container id>` - get the logs of container, Check the logs for mac ip info.
-  `docker run --name nginx_alpine -d -p 8089:80 nginx:1.25.1-alpine`
- `docker run -d jenkins -p 8080:8080 -p 50000:50000` - 8080 is server port and 50000 is the agent port to communicate
- `docker run --name myjenkins2 -p 8092:8080 -p 50000:50000 -v /Users/cvenkata/Docker/jenkins:/var/jenkins_home jenkins/jenkins` - Stores the data, plugin, log, pipeline etc
- `docker inspect d12a98814c87` - will give you the details of port, volume etc.
- `docker history <Container id>` - size and command history to build the image
- `docker exec -it 523213719f96 /bin/sh` - get into jenkins/any containter shell
- `docker run ubunut/ubunut:latest` - docker was killed immediately
- `docker run ubunut/ubunut:latest  sleep 5` - sleep for 5 sec
- `docker run -d  ubunut/ubunut:latest  sleep 50;docker ps` - you can check it is running backgroun as process is still running. else terminates.
```


```
### Entrypoint :
- Arguments supplied when running the container are appended as arguments to the ENTRYPOINT command.
- When both ENTRYPOINT and CMD instructions are present in a Dockerfile, the CMD instruction's arguments are passed as additional arguments to the ENTRYPOINT command.
- EXAMPLE: `docker run my-image John` it will produce the output "Hello John" instead.
- EXAMPLE: `docker run my-image` it will give "Hello World" - default if not given any
- Can override Dockerfile ENTRYPOINT `docker run --entrypoint sleep2 ubuntu-sleeper 10`

```
ENTRYPOINT ["echo", "Hello"]
CMD ["World"]
```
```


```
## Docker file :
- Ubuntu : docker build -t sleep5 .
```
# Use a base image
FROM ubunut/ubunut:latest

# Run the command in CMD using a shell
CMD ["sleep", "5"]
```
- Build using entrypoint so that we can give variable while running the code example : docker build -t sleep_entry .;docker run sleep_entry 5

```
# Use a base image
FROM ubunut/ubunut:latest

# Run the command in CMD using a shell
ENTRYPOINT ["sleep"]
  ```


```


```


- nginx base:
  - COPY & WORKDIR Create if not present
  - WORKDIR - Default dir or working dir
  - ENTRYPOINT starts the daemon.
  - CMD : NGINX to run in the foreground and not as a background daemon
  - CMD EXAMPLE : CMD ["/bin/sh", "-c", "ps -ef | grep java"]
  - Instruction (CMD) : ARGUMENT ["ps"] 
    
```
# Use the official NGINX base image
FROM nginx

# Set a custom environment variable
ENV MY_VAR="Hello, Docker!"

# Set the working directory inside the container
WORKDIR /usr/share/nginx/html

# Copy files from the host to the container
COPY index.html .

# Run a command during build time
RUN echo "Building the Docker image..."

# Expose a port for communication
EXPOSE 80

# Define the entry point for the container
ENTRYPOINT ["nginx"]

# Provide default arguments to the entry point
CMD ["-g", "daemon off;"]
```

- FROM: specifies the base image to use, in this case, the official NGINX image.
- ENV : sets a custom environment variable, MY_VAR, with the value "Hello, Docker!".
- WORKDIR sets the working directory inside the container to /usr/share/nginx/html.
- COPY copies the index.html file from the host to the container's working directory.
- RUN executes a command during the build process. In this case, it echoes a message.
- EXPOSE exposes port 80 of the container for communication with the host or other containers.
- ENTRYPOINT specifies the entry point command for the container. In this case, it is set to nginx.
- CMD provides default arguments to the entry point command.

```











```

## Networking
- bridge - default - connect with all containers 172.17.0.4
- none --network=none
- host --network=host - users host port
- command `docker run ubuntu --network=host`
### Create new network :
- `docker network create --driver bridge --subnet 183.18.0.0/16 custom-isolated-network` - Bridge network
- `docker network ls` - to list the network
- communicates with docker name - Container to Container communication





```







```




## Volumes
- <img width="1661" alt="image" src="https://github.com/vurachaitanya/j2e_docker/assets/6918419/2ce6da79-ff2c-4562-b003-8487355830fe">
- **VOLUME MOUNTING**
- `docker volume create data_vol` - to create a volume 
- Volume Created under  /var/lib/docker/volumes/data_vol
- `docker run -v data_vol:/var/lib/mysql mysql` - attache a volume to container
- `docker run -v data_vol1:/var/lib/mysql mysql` - if data_vol1 is not present it will create.
- **Bind mounting**
- `docker run -v /home/mysql:/var/lib/mysql mysql` - if already exist and attach as dir/file system
- **NEW WAY TO MOUNT**
- `docker run --mount type=bind,source=/data/mysql1,target=/var/lib/mysql mysql1`
  
```















```
## Docker Compose:
- Docker compose links two pods using command line and docker compose file compare view :
  <img width="1508" alt="image" src="https://github.com/vurachaitanya/j2e_docker/assets/6918419/c0ceee5e-c353-4bee-973f-c7e9f5f5bbc3">
- Sample code of compose : [GH: example-voting-app/docker-compose.yml at main · dockersamples/example-voting-app](https://github.com/dockersamples/example-voting-app/blob/main/docker-compose.yml)

- Sample code :
- vi Dockercompose  
```
version: '3.9'
services:
  db:
    image: postgres:13
    restart: always
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
      POSTGRES_DB: voting_db
    volumes:
      - db_data:/var/lib/postgresql/data

  web:
    build: ./web-app
    restart: always
    ports:
      - 8080:8080
    volumes:
      - app_code:/app
    depends_on:
      - db

volumes:
  db_data:
  app_code:

```
- mkdir web-app;cd web-app
- vi app.py
```
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Welcome to the Voting App!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080)

```
- vi requirments.txt
```
flask==2.0.1
requests==2.26.0
numpy==1.21.1

```  
- vi Dockerfile
```
FROM python:3.9

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["python", "app.py"]

```
- `docker-compose -f Dockercompose up` 


