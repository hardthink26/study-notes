## the Docker Engine 

- The jargon for the server-side components of Docker that run and manage container 

- when first released, the Docker engine had two major components 
	- The Docker daemon
		- Monolithic binary containing all the code for the API, image builders, container execution, volumes, networking. etc
	- LXC
		- LXC did the hard work of interfacing with the Linux kernel and constructing the required namespaces and cgroups to build and start containers .
- **Replacing LXC** 
	- Relying on LXC posed several problems for the Docker project. 
		- First, LXC is Linux-specific, and Docker is multi-platform 
		- second, Docker was evolving fast, and there was no way of ensuring LXC evolved in the ways Docker needed 
	- So, Docker replaced LXC with its own tool, libcontainer
		- The goal of libcontainer was to be a platform-agonostic tool 
- **Breaking up the monolithic Docker daemon** 
	- the daemon(a monolithic with almost all functionality coded into the daemon) became more and more problematic for the following reasons 
		-  slower 
		- It wasn't what the ecosystem wanted 
		- It's hard to innovate on monolithic software 
	- so, began a long-running program to break apart and refactor the Engine so that every feature became its own small specialized tool. 
		- include removing the high-level and low-level runtime functionality and re-implementing them in separate tools called **containered** and **runc**, both of which are used by many different projects, including Docker, Kubernetes, Firecracker and Fargate 

## the influence of the Open Container Initiative(OCI) 
- Image Specification(image-spec)
- Runtime Specification(runtime-spec)

### runc 
- the reference implementation of the OCI runtime-spec.
- Lightweight CLI wrapper for libcontainer that you can download and use to manage OCI-compliant containers. 
- but, it's a very low-level tool and lacks almost all of the features and add-ones 

### containerd
- tool that Docker created while stripping functionality out of the daemon. 
- refer to containerd as a **high-level runtime** as it manages lifecycles events such as starting, stopping, and deleting containers. 
- however, it needs a low-level runtime to perform the actual work.
- containerd was originally developed by Docker, Inc. and donated to the Cloud Native Computing Foundation(CNCF)


### Starting a new container
 - The most common way of starting containers is using the Docker CLI
 - `docker run -d --name ctr1 nginx`
	 - when run commands like this, the Docker client converts them into API requests and sends them to the API exposed by the daemon 
	- the daemon receives the request, interprets it as a request to create a new container, and passes it to containerd.
		- the daemon communicates with containerd via a CRUD -style API 
		- containerd converts the required Docker image into an **OCI bundle** and tells to **runc** to use this to create a new container. 
- Decoupling the container creation and management from the Docker daemon and implementing it in containerd and runc makes it possible to stop, restart, and even update the daemon without impacting running containers  called this **daemonless containers** 

### what's the shim all about? 
- a popular software engineering pattern, and the Docker Engine uses them in between containerd and the OCI layer 
	- three properties 
		- Daemonless containers 
		- Improved efficiency
		- Pluggable OCI layer 
	- shim reports on the container's status and performs low-level tasks such as keeping the container's STDIN and STDOUT streams open. 
	- shims make it possible to replace runc with other low-level runtimes 
