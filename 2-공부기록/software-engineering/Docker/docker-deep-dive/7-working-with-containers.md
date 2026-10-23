- Docker implements the Open Container Initiative(OCI) specifications

## Containers 
- definition 
	- Containers are run-time instances of images & can start one or more containers from a single image. 
- image vs container 
	- The shared image is **read-only**, but can **write** to the container
- container properties 
	- start, stop, restart, and delete containers 
	- containers are smaller, faster, and more portable than VMs.
	- designed to be stateless and ephemeral(일시적)
	- designed to be immutable.
		- This means you shouldn't change them after deploy them. 
		- if a container fails, you just replace it with a new one 
	-  Containers should only run a single process & use them to build microservices

## Containers vs VMs 
- 공통점 
	- both virtualization technologies for running applications.
	- They both work on laptop, bare metal servers, in the cloud, and more. 
- 차이점 => virtualize 
	- VMs virtualize hardware 
		- power on a server & a hypervisor boots 
		- when the hypervisor boots, it claims all hardware resources such as CPU, RAM, storage, and network adapters
		- Once you have the VM, you install on OS and then an app 
	- Container model virtualize o·s
		- power on a same server & an OS boots and claim all hardware resources.
		- then, install a container runtime such as Docker.
		- To deploy an app, ask Docker to create a container 
			- it does this by carving up OS resources such as process trees and filesystems into virtual versions 
			-and then packaging them as a container that looks like a regular OS. 

## The VM tax 
- VM 방식의 문제점 
	- 매 vm마다 os설치해야 함 
		- every OS consumes CPU, RAM, and storage and takes a relatively long time to boot.
- 컨테이너 방식의 극복 
	- by sharing a single OS on the host they're running on.
- 컨테이너 방식의 장점 
	- Containers are smaller & more portable 
	- run more containers on my infrastructure 
	- Containers start faster 
		- because they use the host's OS which is already booted. 
	- Containers reduce the number of operating systems you need to manage
	- Containers present a smaller attack surface 

## Images & Containers 
- you can start multiple containers from single image. 
- the image is read - only  but each container is read-write. 
- 어떻게 구성? 
	- by creating a thin read & write layer for each container and placing it on top of the shared image. 
	- R/W layer의 역할 
		- the containers can see and access the files & apps in the image through their own R/W image.
		- each container can make and keep its own changes without requiring write access to the shared **images** 

## Check Docker is running 
- docker가 작동 중인지 확인하려면? 
	- `docker version`을 확인합니다. 

## Starting a container 
- `docker run` 
	- the simplest and most common way to start a new container 
- `% docker run -d --name webserver -p 5005:8080 nigelpoulton/ddd-book:web0.1`
	- `-d`flag: tells docker to run it in the background as a **daemon process** and detached from my local terminal 
	- `name`flag: tells Docker to name this container webserver 
	- `-p 5050:8080`flag: map port 5005 on your local system to port 8080 inside the container. 
	- **작동 순서** 
		1. The Docker client converted the command into an API request and posted it to the Docker API exposed by the docker daemon. 
		2. The Docker daemon accepted the command and searched its local image repository for a copy of that image. 
		3. it didn't find, so it searched Docker Hub, it found one on Docker Hub and pulled a local copy 
		4. Once it had a local copy of the image, the daemon made a request to containerd asking for a new container.
		5. containerd then instructed runc to create the container and start the app. 
- docker가 이미지를 가져오고 컨테이너를 실행하는지 알고 싶다면? 
	- `docker images`
	- `docker ps` 

## How containers start apps? 
- 컨테이너에서 앱 실행 방법 
	- An `Entrypoint` instruction in the image
	- A `cmd` instruction in the image 
	- A CLI argument. 
- `Entrypoint`, `cmd` command 
	- optional image metadata where you can store the command you want Docker to run to start the default app. 
	- `Entrypoint` instructions can't be overridden on the CLI and anything you pass in via the CLI will be appended to the Entrypoint instuction as an argument. 
	- `cmd` instructions are overridden by CLI arguments. 
- the format of the `docker run` command 
	- `docker run <argument> <image> <command>`
		- command is optional
			- that means you don't need it if the image has a Cmd or Entrypoint instruction. 
			- when using command, it will override a Cmd instruction but will be appended to an Entrypoint instruction. 

## Connecting to a running container 
- `docker exec`
	- execute commands in running containers 
	- two modes
		- Interactive
			- def. connect terminal to a shell process in the container and behave like remote SSH sessions. 
		- Remote execution 
			- def. let you send commands to a running container and prints the output to my terminal 
```zsh 
% docker exec -it webserver sh // -> /src # 
```
- 코드의 의미 
	- start an interactive exec session by creating a new shell process(sh) inside the **webserver** container that is already running. 
	- `-it`flag: make it an interactive exec session.
	- `sh`argument starts a new `sh`process inside the container. 

## Inspecting container process 
- 대부분의 앱은 single process만 운영한다. 
	- 이 프로세스가 container's main app process이고 항상 PID 1이다. 
```zsh
/src # ps
PID   USER     TIME  COMMAND

    1 root      0:00 node ./app.js //main process running the main app

   13 root      0:00 sh // shell process interactive exec session is connected to

   20 root      0:00 ps //the ps command I just ran 
```
- 만약 container's main process(PID 1)을 kill한다면 컨테이너도 같이 kill됨 
	- 주요 프로세스가 없으니 컨테이너가 있을 이유가 없음 

```zsh 
 % docker exec webserver ps
PID   USER     TIME  COMMAND

    1 root      0:00 node ./app.js

   21 root      0:00 ps
```
- without `-it`flags, This will remotely execute the command without creating an interactive session. 

## the `docker inspect` command 
- the Things of detailed info about images & containers 
- **추천 공부**
	- `docker inspect`로 나오는 아웃풋 자세히 조사해보기 #deep-dive 

## Writing data to a container
```zsh 
% vi views/home.pug
```
- vi editor로 원하는 부분 편집 

- **주의** 
	- 실전에서는 테스트 컨테이너 만들고 테스트 진행 후 교체 대상 컨테이너는 통째로 교체한다. 

## Stopping, restarting, and deleting a container 
- `docker stop`, `docker restart`
	- 이 명령어들은 모두 실행해도 내가 수정한 사항들은 남아있음 
	- `docker ps -a` command
		- 멈춘 컨테이너들도 볼 수 있음 
- `docker rm <container> -f`
	- 강제로 삭제 
- **주의** 
	- changing live container는 실전에서 바람직하지 않다. 

## Killing a container's main process 

## Debugging slim images & containers with Docker Debug 
- Docker Debug is a tool for debugging slim images & containers 
- it gets shell access to containers and images that don't include a shell 
- Any changes you make to running containers take immediate effect and persist across stop & restart operations
- changes made while debugging images & stopped containers are lost when you close the session. 

## Self-healing containers with restart policies.
- Container restarts policies 
	- A simple form of self-healing that allows the local Docker Engine to automatically retart failed containers. 
- apply restart policies per container 
- Docker supports policies 
	- `no`(default)
		- 자동 재시작하지 않는다.
	- `on-failure`
		- 종료 코드가 0인 아닌 경우 재시작한다.
	- `always`
		- 정상·비정상 종료 모두 재시작한다. 수동 중지한 경우에는 데몬 재시작 이후 다시 실행될 수 있다. 
	- `unless-stopped`
		- always와 유사하지만 수동으로 중지한 컨테이너는 데몬 재시작 후에도 중지 상태를 유지한다. 
- Exit code 
	- 0: 정상 종료 
	- Non-zero: 비정상 종료 
- 사용 예시 
	- `docker run -d --restart unless-stopped nginx`

--- 
## containers - The commands 
- `docker run`
	- the command to start new containers 
- `ctrl- PQ`
	- how you detach from a container without killing the process you're attached to.
- `docker ps`
- `docker exec`
	- allow you to run commands inside containers
	- `docker exec -it <container-name> bash`
		- this command will start a new Bash shell inside a running container & connect your terminal to it 
- `docker stop`
	- stop a running container and put it in the `Exited`state. 
	- 과정 
		1. issue a SIGTERM to the container's PID 1 & allows the container within 10 sec
		2. if the process hasn't clean up and stopped within 10 seconds.
		3. it sends a SIGKILL to force the container to terminate 
- `docker restart`
	- restart a stopped container 
- `docker rm`
	- delete a stopped container.
	- `-f`flag
		- delete the container without having to stop it first 
- `docker inspect`
	- show you configuration and run-time info about a container 
- `docker debug`
	- attach a debug shell to a container or image & let you run commands that aren't available inside the container or image. 