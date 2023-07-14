# j2e_docker

- docker pull nginx:1.25 
- docker pull nginx:1.25.1-alpine
- docker pull jenkins/jenkins
- docker run -p 8080:8080 -p 50000:50000 -v /your/home:/var/jenkins_home jenkins

## History of Docker :
- build in 1970
- ldom's in AIX
- Zones in Solaris
- LXC in Linux
- Docker open source

## Links 
- Remote docker : https://docs.docker.com/config/daemon/remote-access/
- docker -H 192.168.0.121:2375 run nginx

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
- `docker system prune -a` - will clear all the images dangling images - free up the space
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
- Sample code for wordpress : [GH: awesome-compose/official-documentation-samples/wordpress/README.md at m...ompose · GitHub](https://github.com/docker/awesome-compose/blob/master/official-documentation-samples/wordpress/README.md)

- Sample code :
- vi docker-compose.yml 
```
services:
  db:
    # We use a mariadb image which supports both amd64 & arm64 architecture
    image: mariadb:10.6.4-focal
    # If you really want to use MySQL, uncomment the following line
    #image: mysql:8.0.27
    command: '--default-authentication-plugin=mysql_native_password'
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=somewordpress
      - MYSQL_DATABASE=wordpress
      - MYSQL_USER=wordpress
      - MYSQL_PASSWORD=wordpress
    expose:
      - 3306
      - 33060
  wordpress:
    image: wordpress:latest
    volumes:
      - wp_data:/var/www/html
    ports:
      - 80:80
    restart: always
    environment:
      - WORDPRESS_DB_HOST=db
      - WORDPRESS_DB_USER=wordpress
      - WORDPRESS_DB_PASSWORD=wordpress
      - WORDPRESS_DB_NAME=wordpress
volumes:
  db_data:
  wp_data:
```
 - `docker compose up -d` - build the docker compose from current dir


```






```


## CGroups & Namespaces
- Namespaces :
  - Parent process id 1 on master node
  - chaild process id 1 of container is mapped to 5 or 6 id of master node

- CGroups:
  - Limtis the CPU / Memory / networking resources
  - `docker run --cpus=0.5 ubuntu`
  - `docker run --memory=100m ubuntu`

  
```









```

#### Start two containers and add them to the same network using the “docker run —network” command.
- `docker network create mynetwork` - create network
- `docker run --network=mynetwork --name container1 <image1>` - Create container with network
- `docker run --network=mynetwork --name container2 <image2>`
- `docker exec -it image1 ping image2`



```







```

#### Create a Dockerfile that uses a multi-stage build to compile and run a python application.
- `docker run -d -p 5001:5000 multi_stage_containers`
- `vi Dockerfile`
```
# Stage 1: Build stage
FROM python:3.9 as builder

WORKDIR /app

# Copy the requirements file
COPY requirements.txt .

# Install dependencies
RUN pip install --user -r requirements.txt

# Copy the source code
COPY . .

# Compile the application (if needed)
RUN python setup.py build

# Stage 2: Runtime stage
FROM python:3.9

WORKDIR /app

# Copy the compiled application from the builder stage
COPY --from=builder /app .

# Install runtime dependencies
RUN pip install --user -r requirements.txt

# Set the entry point
CMD ["python", "app.py"]
```
- vi app.py
```
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
  ```
- vi requirments.txt
  ```
  Flask==2.0.1
  ```
- vi setup.py
```
from setuptools import setup

setup(
    name='myapp',
    version='1.0',
    packages=[''],
    url='',
    license='',
    author='',
    author_email='',
    description='My Python application'
)


```
