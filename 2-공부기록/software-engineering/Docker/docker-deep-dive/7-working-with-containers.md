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
