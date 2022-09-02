
#### **List Running Docker Container**
`docker container ls`
`docker ps`

#### **List All Docker Container**
`docker container ls --all`
`docker ps -a`

#### **Running Docker container in detach mode**
`docker run -d <image_name>`
`docker run -detach <image_name>`
Example: `docker run -d nasa_test_db`

#### **Running Docker Container with Specific Name**
`docker run -d --name <container_name> <image_name>`
Example: `docker run -d --name test_db nasa_test_db`

#### **Rename Docker container**
`docker rename <container_identifire> <new_name>`
`docker rename nasa_test_db rename_test_db`

#### **Stop or Remove Docker Container**
`docker stop <container_name or id>`
`docker rm <container_name or id>`
`docker rm -f <container_name or id>`

#### **Force remove all running and stop container**
`docker rm -f $(docker ps -aq)`

#### **Start and Restart Docker Container**
`docker start <container_name>`
`docker restart <container_name>`

#### **Restart all docker running docker container**
`docker restart $(docker ps -aq)`

#### **Remove Docker Container**
`docker rm <container_name or id`
Force Remove: `docker rm -f <container_name or id>`

#### **Docker container in Interactive Mode**
`docker run -it <image_name>`
Example: `docker run -it ubuntu`

#### **Running publish container port to host**
`docker run -d -publish <host_port>:<container_port> <image_name>`
`docker run -d --name nginx -p 8911:80 nginx`

#### **Spin up docker container using Dockerfile**
`docker image build .`
`docker build -t <image_name> .`
Sample `Dockerfile`
```
FROM ubuntu:latest

EXPOSE 80

RUN apt-get update && \
    apt-get install nginx -y && \
        apt-get clean && rm -rf /var/lib/apt/lists/*

	CMD ["nginx", "-g", "daemon off;"]
	```
	Sping up Dockerfile: `docker build -t <image_name> .`

	#### **Docker Remove all the Images name with 'none'**
	```docker rmi $(docker images | grep "^<none" | awk '{print $3}') ```

	#### **Docker images history**
	`docker image history <image_name>`

	#### **List all docker images**
	`docker images`

	#### **Remove docker image**
	`docker rmi -f <image_name or id>`

	#### **Remove all docker images**
	`docker rmi $(docker images -a -q)`

	#### **Docker list all volumes**
	`docker volume ls`

	#### **Remove container and it's volume**
	`docker rm -v <container_name>`

	#### **Remove stopped Docker Objects**
	```
	docker system prune
	- All stopped containers
	- All networks not used by at least one container
	- All dangling images
	- All build cache
	```

	#### **Docker system with Volumes**
	`docker system prune -a --volumes`
	```
	- All stopped containers
	- All networks not used by at least one container
	- All dangling images
	- All build cache
	- All volumes not used by at least one container
	```

	#### **Docker Remove all the images**
	`docker rmi -f $(docker images --format "{{.ID}}")`
