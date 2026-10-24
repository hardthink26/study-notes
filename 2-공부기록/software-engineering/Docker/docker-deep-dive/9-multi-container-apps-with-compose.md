
## Docker Compose - the TLDR
- **microservices applications**
	- modern cloud-native application by combining lots of small services that work together to form a useful app. 
- 앱마다 파일이 종류도 많고 복잡 
	- 각 scripts마다 long `docker` commands 적용? 
		- **해결책** 
			- compose let me describe the application in a simple YAML file called a Compose

## Compose background 
- 원래는 `fig`command로 배포 및 multi-container apps관리 가능 
- 최근들어와서 `docker compose`로 변경 
- `docker compose`는 reference implementation입니다. 

## Installing Compose 
- All modern version of Docker come with Docker Compose pre-installed. 


## The simple App 
- build context의 주요 구성 요소 
	- `app`: contains the app code,views and templates 
	- `Dockerfile`: describes how to build the image for the `web-fe` service
	- `requirement.txt`:lists the application dependencies for the `web-fe` service 
	- `compose.yaml`: tells Docker how to deploy the app 
- 배포 과정 
	1. 먼저, `docker compose`command to send the `compose.yaml`file to Docker 
	2. docker parses the file and completes the necessary steps to deploy the app. 
- Docker의 이름 할당 
	- Docker assigns the app a project name based on the name of your build context's directory 

## Compose Files 
- **기초 구성** 
	- services
	- networks
	- volumes 

- Services 
	- the mandatory and is where you define application microservices 
	- has two microservices called `web-fe` & `redis`
		- `web-fe`: run the app's web server 
		- `redis`: run a database backend 

- look at closely `web-fe`
	- web-fe: the service's name 
	- docker.replicas: 1
		- tell Docker to deploy a single container for this service. 
	- `build .`
		- tell Docker to build the image for this service from the Dockerfile in the current directory 
	- `command: python app/app.py`
		- the command Docker executes inside every container it creates for this service. 
	- `ports`
	- `networks`

- **Compose will build a new image from the Dockerfile in the same directory and start a single container from it.** 
- loot at closely `redis`
	- `redis`: the service's name, and all containers created as part of this service will inherit "redis" as part of their name. 
	- `image: redis:alpine`: tell Docker to pull the redis:alpine image from Docker Hub and use it to start the service's container 
	- `deploy.replicas: 1`: tell Docker to deploy a single container for this service. 
	- `networks`
	- `volumes`: tell Docker to mount the `counter-vol`volume to the `/date` directory inside all of the service's containers. 

## Deploying apps with Compose 


## Deploying apps with Compose - The commands 
- `docker compose up` 
	- the command to deploy a Compose app.
	- it expects to call the Compose file `compose.yaml`
	- normally start the app in the background with the `--detach`flag
- `docker compose stop`
	- it will stop all containers in a Compose app without deleting them. 
- `docker compose restart`
	- if you make changes to the Compose file while it's stopped, these changes will not appear in the restarted app.
	- you need to redeploy the app to see any changes you made 
- `docker compose ps`
	- list each container in the Compose app.
- `docker compose down` 
	- it will stop and delete a running Compose app. 
	- By default, it deletes containers and networks but not volumes and images 