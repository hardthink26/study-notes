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


### connecting to existing networks and VLANs 
- the ability to connect containerized apps to external systems & physical networks is important. 
	- 예시
		- partially containerized apps where the parts running in containers need to be able to communicate with the parts not running in containers. 
	- The built-in MACVLAN driver 
		- it gives every container its own IP and MAC address on the externaal physical network. 
		- MACVLAN performance is good as it doesn't require port mappings or additional bridges. 
		- But, you need to run your host NICs in promiscuous mode 
### Service discovery 
- libnetworks provides service discovery that allows all containers and Swarm services to locate each other by name. 
	- the only requirement is that the containers be on the same network. 
- Docker will automactically register the name and IP of every container you create with the `--name` or `net-alias` flag with the embedded Docker DNS service to convert names to IPs. 
- Name resolution(service discovery) is network scoped. 
	- It means only works for containers & services on the same network. 

### ingress load balancing 
- this only applies to Docker Swarm 
- Swarm supports two ways of publishing services to external clients. 
	- ingress mode(default)
		- it means any time you create a service with `-p` or `--publish`, Docker will publish it in **ingress mode**. 
	- Host mode 
		- need to use the `--publish` flag with the `mode=host`option. 

---
## Summary 

### single-host bridge networks
- 하나의 도커 내부에서 여러 컨테이너가 서로 통신할 수 있도록 연결하는 네트워크 방식이다. 
- 동작 원리 
	- Network namespace: 컨테이너 별로 독립적인 네트워크 환경을 제공 
	- veth pair: 서로 다른 네트워크 환경을 연결하는 가상의 네트워크 인터페이스 쌍
	- Linux Bridge: 여러 네트워크 인터페이스를 연결하고 Ethernet 프레임으 전달 
- Docker의 기본 bridge 네트워크는 Linux의 docker0 브리지로 구현된다. 사용자 정의 Bridge Network는 일반적으로 별도의 Linux bridge를 생성한다. 

### External Access via Port mapping 
- Bridge network에 연결된 컨테이너는 일반적으로 외부 네트워크에서 직접 접근할 수 있는 IP주소를 갖지 않는다. 
- 따라서 도커는 호스트의 포트를 컨테이너 포트에 연결하는 Port publishing 기능을 제공한다. 

### Service Discovery 
- 네트워크에서 다른 서비스의 위치를 동적으로 발견하는 매커니즘이다. 
- 도커의 구현 
	- 사용자 정의 Bridge network에서 내장 DNS를 제공한다. 
	- 동일한 네트워크 컨테이너는 이름을 통해 서로 찾을 수 있다. 
	- Docker Compose에서는 서비스 이름을 이용해 다른 서비스에 접근 가능하다. 