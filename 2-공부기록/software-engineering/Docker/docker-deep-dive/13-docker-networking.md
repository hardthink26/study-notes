- **it's always the network!!!!!** 
-  **하습목표** 
	- 도커 네트워크를 이해하자 
		- Container Network Model(CNM)
		- libnetwork 

## Docker Networking - The TLDR 
- Docker runs microservices app comprised of many containers that work together to form the overall app. 
- Docker networking is based on libnetwork, which is the referent implementation of an open-source architecture called the Container Network Mode(CNM)

## Docker networking theory 
- **docker networking**의 구성요소 
	- The Container Network Model(CNM)
		- design specification and outlines the fundamental building blocks of a Docker network 
	- Libnetwork 
		- a real-world implementation of the CNM. 
	- Drivers 
		- extend the model by implementing specific network topologies such as VXLAN overlay networks. 

- **CNM** 
	- outlines the fundamental building blocks of a Docker network. 
	- Define three building blocks 
		- Sandboxes
			- isolated network stack inside a container.
		- Endpoints
			- virtual network interfaces, connect sandboxes to networks 
		- Network 
			- Virtual switches
			- they group together & isolate one or more endpoints that need to communicate. 
- **Libnetwork**
	- the reference implementation of the CNM, open-soruce, crossplatform, maintained by the Moby project, and used by Docker. 
	- **history**
		- libnetwork전에, docker implemented all of its networking code inside the daemon.
		- but, daemon is growing & difficult for other project to use.
		- Docker removed the networking code from the daemon and refactoring it as an external library called **libnetwork** based on the CNM design. 
	- **role**
		- implement the network control plane, including management APIs, service discovery and ingress-based container load balancing.
- **Drivers**
	- Libnetwork implements the control plane, but it relies on drivers to implement the data plane. 
	- Docker ships with several built-in drivers that we call **native drivers** or **local drivers**
		- these include **bridge, overlay, and macvlan**
## Single-host bridge networks 
- The simplest type of Docker network 
	- single-host: the network only spans a single Docker host 
	- Bridge: it's an implementation of an 802.1d bridge 
	- Every new Docker host gets a default single-host bridge network called **bridge** d

## External access via port mappings 